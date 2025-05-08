# Audio communication protocol between client and server

This document describes the rules for communication between a client and a server for exchanging audio. At the time of writing, video communication is not supported.

This protocol is intentionally made very simple, you don't want to obfuscate simple things. It can also be changed as needed.

This document is written more for the convenience of developers rather than for public use.

### 1. Handshake
The client always initiates the data transfer. At this point, the client says that it wants to establish a connection with a specific user and characteristics. The packet looks like this(Message Type 0):
| Offset (bytes) | Length (bytes) | Field Name        |
|----------------|----------------|-------------------|
| 0              | 4              | Length            |
| 4              | 2              | Message Type      |
| 6              | 4              | Sample Rate (Hz)  |
| 10             | 2              | Channels Count    |
| 12             | 1              | Format Type       |
| 13             | 4              | Chunk Size        |
| 17             | 2              | Username Length   |
| 19             | N              | Username          |
| 19 + N         | M              | Optional Data     |

Server is an intermediary between two clients, in this case it will check if such a user exists and send him a request. If user does not exists(Message Type 1), packet will be:
| Offset (bytes) | Length (bytes) | Value             | Field Name        |
|----------------|----------------|-------------------|-------------------|
| 0              | 4              | 6                 | Length            |
| 4              | 2              | 0                 | Message Type (Hz) |

In other case server sends request to other client, it should check if its parameters are satisfactory, and if not(Message Type 2), it should send a response to the server, in which it specifies the desired parameters:
| Offset (bytes) | Length (bytes) | Field Name        |
|----------------|----------------|-------------------|
| 0              | 4              | Length            |
| 4              | 2              | Message Type      |
| 6              | 4              | Sample Rate (Hz)  |
| 10             | 2              | Channels Count    |
| 12             | 1              | Format Type       |
| 13             | 4              | Chunk Size        |
| 17             | N              | Optional Data     |

If the client is not satisfied with such parameters, he should close the connection(Message Type 3):
| Offset (bytes) | Length (bytes) | Field Name        |
|----------------|----------------|-------------------|
| 0              | 4              | Length            |
| 4              | 2              | Message Type      |
| 6              | N              | Optional Data     |

But if the client is satisfied with the parameters, it sends the following packet(Message Type 4):
| Offset (bytes) | Length (bytes) | Field Name        |
|----------------|----------------|-------------------|
| 0              | 4              | Length            |
| 4              | 2              | Message Type      |
| 6              | N              | Optional Data     |

But it's also possible for a client to drop a call without reason(Message Type 5):
| Offset (bytes) | Length (bytes) | Field Name        |
|----------------|----------------|-------------------|
| 0              | 4              | Length            |
| 4              | 2              | Message Type      |
| 6              | N              | Optional Data     |

Data packet looks like:
But it's also possible for a client to drop a call without reason(Message Type 6):
| Offset (bytes) | Length (bytes) | Field Name        |
|----------------|----------------|-------------------|
| 0              | 4              | Length            |
| 4              | 2              | Message Type      |
| 6              | N              | Data              |

Format type values:
```
  uint8 = 0
  int8 = 1
  int16 = 2
  int32 = 3
  float32 = 4
```
