### Function of Layer 4 (Transport Layer)

- Provides transparent transfer of data between end hosts.

- Provides (or doesn't provide) various services to applications:
- reliable data transfer
- error recovery
- data sequencing
- flow control

- Provides Layer 4 addressing (port number).
- Identify the application layer protocol
- Provides session multiplexing.
- The following range of port numbers are used for different purposes:
  - 0-1023: well-known ports (assigned to specific services, e.g., HTTP uses port 80, HTTPS uses port 443)
  - 1024-49151: registered ports (can be registered for specific applications)
  - 49152-65535: dynamic/private ports (used for temporary purposes, often assigned dynamically by the operating system)

### TCP [YT](https://youtu.be/LIEACBqlntY?si=lSWF0lQ9kWLaZnfg&t=922)

- Connection-oriented protocol.
  Before data transfer, a connection must be established between the sender and receiver using a three-way handshake (SYN, SYN-ACK, ACK).

- TCP provides reliable communication.
  The destination host must acknowledge that it received each TCP segment.
  If a segment isn't acknowledge, it is sent again.

- TCP provides sequencing.
  Each byte of data is assigned a sequence number, and the receiver uses these numbers to reassemble the data in the correct order.

- TCP uses forward acknowledgments, meaning that the acknowledgment number in a TCP segment indicates the next expected byte from the sender.

| Field                                      | Size     |
| ------------------------------------------ | -------- |
| Source Port                                | 16 bits  |
| Destination Port                           | 16 bits  |
| Sequence Number                            | 32 bits  |
| Acknowledgment Number                      | 32 bits  |
| Data Offset                                | 4 bits   |
| Reserved                                   | 3 bits   |
| Flags (NS,CWR,ECE,URG,ACK,PSH,RST,SYN,FIN) | 9 bits   |
| Window Size                                | 16 bits  |
| Checksum                                   | 16 bits  |
| Urgent Pointer                             | 16 bits  |
| Options + Padding                          | Variable |

### UDP

- Connectionless protocol.
  No need to establish a connection before sending data.
- UDP does not guarantee reliable communication.
  The sender does not wait for acknowledgments from the receiver, and there is no mechanism for retransmission of lost packets.
- UDP does not provide sequencing.
  Each datagram is independent, and there is no inherent order to the data.

| Field            | Size    |
| ---------------- | ------- |
| Source Port      | 16 bits |
| Destination Port | 16 bits |
| Length           | 16 bits |
| Checksum         | 16 bits |


