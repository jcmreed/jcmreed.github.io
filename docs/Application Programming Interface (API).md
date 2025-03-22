---
title: Application Programming Interface (API)
---

### __My role in the Daisy Chain__
Each team member has their own responsibility on what to send to the chain and how to handle necessary messages from the chain. Again, the subsystem I am responsible for is the Human Machine Interface (HMI). The HMI is where the exhibit user will select a path that they believe matches the color of the element. Listed below is the messaging protocol that the team is using throughout this process. For a more detailed explanation, please refer to this [link.](https://asu-egr314-2025-s-201.github.io/04-Block%20Diagram%2C%20Process%20Diagram%2C%20and%20Message%20Structure/)

### __Team Message Protocol__
|Message Type <br> byte 1-2 <br>(uint16_t) | Description|
|-------------------|---------------|
|0                  | Status Code   |
|1                  | Drive Mode    |
|2                  | Sensor Data   |
|3                  | Path Selection|

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