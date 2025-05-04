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
| ALL | Broadcast | X |

### __Team Message Protocol__
|Message Type <br> byte 1 <br>(uint8_t) | Description| Data Command |
|-------------------|---------------| -------------------------- |
|0                  | Status Code   | 0 (Initialize) | 
|1                  | Drive Mode    | 0 (Automatic) <br> 1 (Manual/Direct Drive) |
|2                  | Sensor Data   | 0 (Orange) <br> 1 (Blue) <br> 2 (Pink) |
|3                  | Path Selection| 0 (Left) <br> 1 (Center) <br> 3 (Right) |

My  Human Machine Interface uses 3 of these message types. The first one is the status code, which essentially tells the next system in the chain that my system is initialized.

### __Message Type 0: Initialization__
|         |  Byte 1  | Byte 2 | 
|---------|----------|---------|
|Var Name | msg_type | initialize  |
|Var Type | uint8_t | uint8_t |
|Min Val  | 0        | 0       | 
|Max Val  | 0        | 0       |
|Example  | 0        | 0       |

The second message type that my subsystem uses is Message Type 1: Select Mode. This is because when the exhibit user interacts with the HMI, a message must be sent to the chain that the entire system should enter Direct Drive (Manual) Mode. Also, if the user does not interact with the device for a certain amount of time, then the message for autonomous mode is sent.

### __Message Type 1: Select Mode__
|         |  Byte 1  | Byte 2 | 
|---------|----------|---------|
|Var Name | msg_type | mode  |
|Var Type | uint8_t | uint8_t |
|Min Val  | 1        | 0       | 
|Max Val  | 1        | 1       |
|Example  | 1        | 1       |

The final message type that my subsystem uses is Message Type 3: Select Path. The user must make a path choice on the HMI, and depending on their selection, that's what tells the Actuator where to move to.

### __Message Type 3: Select Path__
|         |  Byte 1  | Byte 2 |
|---------|------------|--------|
|Var Name | msg_type   | path   |
|Var Type | uint8_t   | uint8_t|
|Min Val  | 3          | 0      |
|Max Val  | 3          | 2      |
|Example  | 3          | 1      |

### __Handling Messages (Code)__

```python
# --- Imports ---
from machine import UART, Pin
import time
import uasyncio as asyncio

# --- Constants and Configuration ---
MAX_MESSAGE_LEN = 64  # Max message length to protect buffer overflow
team = [b'H', b'M', b'A', b'S']  # Valid team device IDs
id = b'H'                        # This device's ID
broadcast = b'X'                # Broadcast ID for sending to all devices

# Message type definitions and valid data ranges
VALID_MESSAGE_TYPES = {
    0x00: [0x00],                    # Status Code
    0x01: [0x00, 0x01],              # Drive Mode
    0x02: [0x00, 0x01, 0x02],        # Sensor Data
    0x03: [0x00, 0x01, 0x02]         # Path Selection
}

# --- Hardware Initialization ---
# UART2 with TX on GPIO17, RX on GPIO18
uart = UART(2, 9600, tx=17, rx=18)
uart.init(9600, bits=8, parity=None, stop=1)

# Onboard LED for visual feedback (GPIO 7)
led = Pin(7, Pin.OUT)

# --- Message Sending Function ---
def send_message(sender, receiver, msg_type, data):
    # Validate sender, receiver, type, and data
    if sender not in team:
        print("Error: Invalid sender ID.")
        return
    if receiver not in team and receiver != broadcast:
        print("Error: Invalid receiver ID.")
        return
    if msg_type not in VALID_MESSAGE_TYPES or data not in VALID_MESSAGE_TYPES[msg_type]:
        print("Error: Invalid message type or data value.")
        return

    # Construct and send the UART message
    message = b"AZ" + sender + receiver + bytes([msg_type]) + bytes([data]) + b"YB"
    uart.write(message)
    print(f"Sent: {message}")

# --- Message Receiving and Handling Function ---
def handle_message(message):
    try:
        # Basic message size validation
        if len(message) < 7:
            print("ESP: Message too short, deleting.")
            return
        if len(message) > MAX_MESSAGE_LEN:
            print("ESP: Message too long, deleting.")
            return

        # Parse message structure
        prefix = message[:2]
        sender = message[2:3]
        receiver = message[3:4]
        msg_type = message[4] 
        data = message[5] 
        suffix = message[-2:]

        # Check for proper framing
        if prefix != b"AZ" or suffix != b"YB":
            print("ESP: Invalid message format, deleting.")
            return

        # Validate sender and receiver
        if sender not in team:
            print(f"ESP: Invalid sender {sender}, deleting.")
            return
        if receiver not in team and receiver != broadcast:
            print(f"ESP: Invalid receiver {receiver}, deleting.")
            return

        # Validate message type and associated data
        if msg_type not in VALID_MESSAGE_TYPES:
            print(f"ESP: Invalid message type {msg_type}, deleting.")
            return
        if data not in VALID_MESSAGE_TYPES[msg_type]:
            print(f"ESP: Invalid data value {data} for message type {msg_type}, deleting.")
            return

        # Drop message if it's from self
        if sender == id:
            print("ESP: Deleted own message.")
            return

        # Act on messages addressed to this device or broadcast
        if receiver == id or receiver == broadcast:
            print(f"ESP: Received message from {sender.decode()}! Type {msg_type}, Data {data}")
            led.value(led.value() ^ 1)  # Toggle LED for activity feedback

        # Forward messages not intended for this device
        if receiver != id and receiver != broadcast:
            print(f"ESP: Forwarding message to {receiver.decode()}")
            uart.write(message)

    except Exception as e:
        print(f"ESP: Error processing message: {e}")

# --- Asynchronous Task: UART Receiver ---
async def process_rx():
    stream = b''              # Buffer for collecting bytes
    receiving_message = False # Flag for message collection state

    while True:
        c = uart.read(1)  # Read 1 byte at a time

        if c:
            stream += c

            # Detect message start
            if stream[-2:] == b'AZ':
                receiving_message = True
                stream = b'AZ'  # Reset buffer with prefix only

            # Detect complete message
            if receiving_message and stream[-2:] == b'YB':
                receiving_message = False
                handle_message(stream)
                stream = b''

            # Drop overlong messages
            if len(stream) > MAX_MESSAGE_LEN:
                print("ESP: Message too long, deleting.")
                stream = b''
                receiving_message = False

        await asyncio.sleep_ms(10)

# --- Asynchronous Task: Heartbeat / Test Ping ---
async def heartbeat():
    while True:
        print('ESP: Sending Test Code')
        # Uncomment below to send a sample message for testing
        # send_message(b'S', id, 0x02, 0x02)
        # uart.write(b'AZHX\x00\x03\x03YB')
        await asyncio.sleep(10)

# --- Main Loop (placeholder for future tasks) ---
async def main():
    while True:
        await asyncio.sleep(1)

# --- Task Scheduling ---
asyncio.create_task(process_rx())     # Start UART receiver
asyncio.create_task(heartbeat())      # Start heartbeat sender

# --- Run Event Loop ---
try:
    asyncio.run(main())
finally:
    asyncio.new_event_loop()  # Reset event loop if interrupted
```