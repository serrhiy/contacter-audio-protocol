# Audio communication protocol between client and server

This document describes the rules for communication between a client and a server for exchanging audio. At the time of writing, video communication is not supported.

This protocol is intentionally made very simple, you don't want to obfuscate simple things. It can also be changed as needed.

This document is written more for the convenience of developers rather than for public use.

### 1. Handshake
The client always initiates the data transfer. At this point, the client says that it wants to establish a connection with a specific user and characteristics. The packet looks like this
| Offset (bytes) | Length (bytes) | Field Name        |
|----------------|----------------|-------------------|
| 0              | 4              | Length            |
| 4              | 2              | Message Type (Hz) |
| 6              | 4              | Sample Rate       |
| 10             | 2              | Channels Count    |
| 12             | 1              | Format Type       |
| 13             | 4              | Chunk Size        |
| 17             | 2              | Username Length   |
| 19             | N              | Username          |
| 19 + N         | M              | Optional Data     |
