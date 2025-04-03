---
title: Application Programming Interface (API)
---

### __My role in the Daisy Chain__
Each team member has their own responsibility on what to send to the chain and how to handle necessary messages from the chain. Again, the subsystem I am responsible for is the Human Machine Interface (HMI). The HMI is where the exhibit user will select a path that they believe matches the color of the element. Listed below is the messaging protocol that the team is using throughout this process. For a more detailed explanation, please refer to this [link.](https://asu-egr314-2025-s-201.github.io/04-Block%20Diagram%2C%20Process%20Diagram%2C%20and%20Message%20Structure/)

### __Address IDs__
| Team Member | Subsystem | ID |
| ----------- | --------- | -- |
| JC R (myself) | Human Machine Interface (HMI) | H |
| Eric M | RGB Sensor | S |
| Marcus P | MQTT Server | M |
| Bradley P | Actuator | A |

### __Team Message Protocol__
|Message Type <br> byte 1-2 <br>(uint16_t) | Description| Data Command |
|-------------------|---------------| -------------------------- |
|0                  | Status Code   | 0 (Offline) <br> 1 (Online) <br> 2 (Waiting) <br> 3 (Error) | 
|1                  | Drive Mode    | 0 (Automatic) <br> 1 (Manual/Direct Drive) |
|2                  | Sensor Data   | 0 (Orange) <br> 1 (Blue) <br> 2 (Pink) |
|3                  | Path Selection| 0 (Left) <br> 1 (Center) <br> 3 (Right) |

The Human Machine Interface uses 3 of these message types. The first one is the status code, which essentially tells the next system in the chain that my system is online/offline. 

### __Message Type 0: Status Code__
|         |  Byte 1-2  | Byte 3 | 
|---------|----------|---------|
|Var Name | msg_type | status  |
|Var Type | uint16_t | uint8_t |
|Min Val  | 0        | 0       | 
|Max Val  | 3        | 3       |
|Example  | 0        | 2       |

The second message type that my subsystem uses is Message Type 1: Select Mode. This is because when the exhibit user interacts with the HMI, a message must be sent to the chain that the entire system should enter Direct Drive (Manual) Mode. 

### __Message Type 1: Select Mode__
|         |  Byte 1-2  | Byte 3 | 
|---------|----------|---------|
|Var Name | msg_type | mode  |
|Var Type | uint16_t | uint8_t |
|Min Val  | 0        | 0       | 
|Max Val  | 3        | 1       |
|Example  | 1        | 1       |

The final message type that my subsystem uses is Message Type 3: Select Path. The user must make a path choice on the HMI, and depending on their selection, that's what tells the Actuator where to move to.

### __Message Type 3: Select Path__
|         |  Byte 1-2  | Byte 3 |
|---------|------------|--------|
|Var Name | msg_type   | path   |
|Var Type | uint16_t   | uint8_t|
|Min Val  | 0          | 0      |
|Max Val  | 3          | 2      |
|Example  | 3          | 2      |

### __Handling Messages (Code)__

```python
#Import Modules
from machine import UART
from machine import Pin
import time
import uasyncio as asyncio

MAX_MESSAGE_LEN=64
team = [b'H',b'M',b'A',b'S']
id = b'H'
broadcast = b'X'

VALID_MESSAGE_TYPES = {
    0x00: [0x00, 0x01, 0x02, 0x03],  # Status Code (0-3)
    0x01: [0x00, 0x01],              # Drive Mode (0-1)
    0x02: [0x00, 0x01, 0x02],        # Sensor Data (0-2)
    0x03: [0x00, 0x01, 0x02]         # Path Selection (0-2)
}

# initialize a new UART class
uart = UART(2, 9600,tx=37,rx=36)

# run the init method with more details including baudrate and parity
uart.init(9600, bits=8, parity=None, stop=1) 

# define pin 7 as an output with name led.
led = Pin(7,Pin.OUT)

def send_message(sender, receiver, msg_type, data):
    if sender not in team: 
        print("Error: Invalid sender ID.")
        return
    if receiver not in team and receiver != broadcast:
        print("Error: Invalid receiver ID.")
        return
    if msg_type not in VALID_MESSAGE_TYPES or data not in VALID_MESSAGE_TYPES[msg_type]:
        print("Error: Invalid message type or data value.")
        return
    
    message = b"AZ" + sender + receiver + msg_type.to_bytes(2, 'big') + data.to_bytes(1, 'big') + b"YB"
    
    uart.write(message)
    print(f"Sent: {message}")

def handle_message(message):
    try:
        if len(message) < 7:
            print("ESP: Message too short, deleting.")
            return
        
        if len(message) > MAX_MESSAGE_LEN:
            print("ESP: Message too long, deleting.")
            return

        prefix = message[:2]
        sender = message[2:3]
        receiver = message[3:4]
        msg_type = int.from_bytes(message[4:6], 'big')
        data = int.from_bytes(message[6:7], 'big')
        suffix = message[-2:]

        if prefix != b"AZ" or suffix != b"YB":
            print("ESP: Invalid message format, deleting.")
            return
        
        if sender in team:
            print(f"ESP: Valid sender: {sender.decode()}")
        else:
            print(f"ESP: Invalid sender {sender}, deleting.")
            return
        
        if receiver in team or receiver == broadcast:
            print(f"ESP: Valid receiver: {receiver.decode()}")
        else:
            print(f"ESP: Invalid receiver {receiver}, deleting.")
            return
        
        if msg_type not in VALID_MESSAGE_TYPES:
            print(f"ESP: Invalid message type {msg_type}, deleting.")
            return
        
        if data not in VALID_MESSAGE_TYPES[msg_type]:
            print(f"ESP: Invalid data value {data} for message type {msg_type}, deleting.")
            return
        
        # Self-message check AFTER validation
        if sender == id:
            print("ESP: Deleted own message.")
            return

        # If message is valid and meant for this device
        if receiver == id or receiver == broadcast:
            print(f"ESP: Received message from {sender.decode()}! Type {msg_type}, Data {data}")
            led.value(led.value() ^ 1)  # Toggle LED

        # Forward message if not intended for this device
        if receiver != id and receiver != broadcast:
            print(f"ESP: Forwarding message to {receiver.decode()}")
            uart.write(message)

    except Exception as e:
        print(f"ESP: Error processing message: {e}")
    
async def process_rx():
    stream = b''  # Buffer to collect incoming message
    receiving_message = False

    while True:
        c = uart.read(1)  # Read one byte

        if c:
            stream += c  # Append byte to stream

            if stream[-2:] == b'AZ':  # Message start detected
                receiving_message = True
                stream = b'AZ'  # Reset stream, keeping 'AZ'

            if receiving_message and stream[-2:] == b'YB':  # Message end detected
                receiving_message = False
                handle_message(stream)  # Process the full message
                stream = b''  # Reset buffer for next message

            if len(stream) > MAX_MESSAGE_LEN:  # Prevent oversized messages
                print("ESP: Message too long, deleting.")
                stream = b''  # Clear buffer
                receiving_message = False

        await asyncio.sleep_ms(10)  # Small delay for async handling   

async def heartbeat():
    while True:
        print('ESP: Sending Test Code')
        send_message(id, b'A', 0x02, 0x01)
        #uart.write(b'AZHX\x00\x03\x03YB')
        await asyncio.sleep(10)

async def main():
    while True:
        await asyncio.sleep(1)

asyncio.create_task(process_rx())
asyncio.create_task(heartbeat())

try:
    asyncio.run(main())
finally:
    asyncio.new_event_loop()
```