### FTP & TFTP

- FTP and TFTP (Trivial File transfer protocol) are industry standard protocols used to transfer files over a network.
- They both use a client-server mdoel.
  - Clients can use FTP or TFTP to copy files from a server.
  - Clients can use FTP or TFTP to copy files to a server.

- As a network engineer, the most common use for FTP/TFTP is in the process of upgrading the operating system of a network device.
- You can use FTP/TFTP to download the newer version of IOS from a server, and then reboot the device with the new IOS image.

### TFTP 
- TFTP was first defined in 1981.
- Named "trivial" because it is a very simple protocol with no authentication, no encryption, and no error recovery mechanisms.
- Was released after FTP, but is not a replacement for FTP. It is another tool to use when lightweight simlicity is more important than functionality. 
- No auth (username/password), so servers will respond to all TFTP requests. 
- No encryption, so all data is sent in plain text.
- Best used in a controlled environment to transfer small files quickly. 
- TFTP servers listen on UDP *port 69*. 
- UDP is connectionless and doesn't provide reliablit with retransmissions. 
- HOwever, TFTP has similar built-in features within the protocol itself. 

#### TFTP Reliability
- Every TFTP data message is acknowledged. 
  - If the client is transferring a file to the server, the server will send Ack messages. 
  - If the server is transferring a file to the client, the client will send Ack messages. 
- Timers are used, and if an expected message isn't received in time, the waiting device will re-send its previous message. 

#### TFTP Connections
- TFTP file transfers have three phases: 
 1. Connection: TFTP client sends a request to the server, and the server responds back, initializing the connection. 
 2. Data Transfer: The client and server exchange TFTP messages. One sends data and the other sends acknoweledgments. 
 3. Connection Termination: After the last data message has been sent, a final acknoweledgments is sent to terminate the connection. 

#### TFTP TID
- When the client sends the first message to the server, the destination port is UDP 69 and the source is a random ephemeral port. 
- This random port is called a 'Transefer ID' (TID) and identifies the data transfer. 
- The server then also selects a random TID to use as teh source port when it replies, not 69. 
- When the client sends the next message, the destination port will be the server's TID, not 69.

---
 
### FTP 
- FTP was standardized in 1971. 
- FTP uses TCP ports 20 and 21. 
- Usernames nad passowrds are used for authentication, however there is no encryption. 
- For greated security, FTPS (FTP over SSL/TLS) can be used, which adds encryption.
- SSH File Transfer Protocol (SFTP) can also be used for greated security. 
- FTP is more complex than TFTP and allowss not only file transfers, but clients can also navigate file directories, add and remove directories, list fiels, etc. 
- The client sends FTP commands to the server to perform these functions. 

#### FTP control connections 
- FTP uses two types of connections: 
 - An FTP control connection (TCP 21) is established and used to send FTP commands and replies. 
 - An FTP data connection (TCP 20) is established and used to transfer files.

#### Active Mode FTP Data Connections
- The default method of establishing FTP data connections is active mode, in which the *server initialtes the TCP connection*. 

#### Passive Mode FTP Data Connections
- In FTP passive mode, the client initiates the data connection. This is often necessary when the client is behind a firewall, which could block the incoming connetion from the server.
NOTE: Firewalls ususally don't permit 'outside' devices to initiate connections. In this case, FTP passive mode is used and the client (behind firewall) initiates the TCP connection. 

---

### IOS File Systems
- A file system is a way of controlling how data is stored and retrieved. 
- `show file systems` command shows the file systems available on a device.
 - few terms: 
    - `disk0:` is the internal flash memory of the device. 
    - `nvram:` is the non-volatile RAM, which stores the startup configuration file. 
    - `bootflash:` is a type of flash memory used in some devices. 
    - `network` Represents extenral file systems, for example external FTP/TFTP server. 
    - `tftp:` and `ftp:` are virtual file systems that allow you to access files on a TFTP or FTP server.

#### Upgrading CISCO IOS
- To view current version `show version`
- To view contents of flash `show flash`
- To copy new IOS image to flash, use `copy tftp: flash:` or `copy ftp: flash:`
- After copying, use `show flash` to verify the new image is there.
- Enter configuration mode `boot system flash:filename` to set the new image as the default.
- Save the configuration `write memory` or `copy running-config startup-config`
- `reload` the device to boot with the new image.
- `delete [filepath]`

---

#### Copyting Files (FTP)
- in configuration mode, `ip ftp username [username]` and `ip ftp password [password]` to set the FTP credentials.
- `copy ftp: flash:` to copy a file from an FTP server to the device's
