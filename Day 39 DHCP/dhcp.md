### Purpose:

- Used to allow hosts to automatically/dynamically learn various network config:
  - IP address
  - Subnet mask
  - Default gateway
  - DNS server
- Typically used for "client devices" such as PCs, phones, etc.
- Devices such as routers, server, etc, are usually manually configured.
- In small networks (such as home networks) the router typically acts as the DHCP server for hosts in the LAN.
- In larger networks, the DHCP server is usually a Windows/Linux server.

BONUS: We can check if DHCP is enabled on windows by `ipconfig /all` and looking for "DHCP Enabled: Yes".

### DHCP Release

- DHCP client can release its DHCP configured address by sending a DHCP release message to the DHCP server.

#### Releasing DHCP configured address:

`ipconfig /release` on Windows, `dhclient -r` on Linux

### DHCP Renew

- DHCP client can renew its DHCP configured address by sending a DHCP request message to the DHCP server
- Process:
  1. DHCP Discover:
  - DHCP client broadcasts a DHCP Discover message to find available DHCP servers on the network.
  2. DHCP Offer:
  - DHCP servers that receive the DHCP Discover message respond with a DHCP Offer message, which includes an available IP address and other network configuration parameters.
  3. DHCP Request:
  - The DHCP client sends a DHCP Request message to the DHCP server that made the offer, indicating that it wants to use the offered IP address.
  4. DHCP Acknowledgment:
  - The DHCP server responds with a DHCP Acknowledgment message, confirming that the client can

| Phase    | Direction       | Transmission Type    |
| -------- | --------------- | -------------------- |
| Discover | Client → Server | Broadcast            |
| Offer    | Server → Client | Broadcast or Unicast |
| Request  | Client → Server | Broadcast            |
| Ack      | Server → Client | Broadcast or Unicast |
| Release  | Client → Server | Unicast              |

#### Renewing DHCP configured address:

`ipconfig /renew` on Windows, `dhclient` on Linux

### DHCP Relay: 
NOTE: Broadcast messages don't leave the local subnet. 

- Some network engineers might choose to configure each router to act as the DHCP server for its connected LANs. 
- However, large enterprises  often choose to use a centralized DHCP server. 
- If the server is centralized, it wond't receive the DHCP clients broadcast DHCP messages. 
- To fix this, you can fconfigure a router to act as a *DHCP relay agent*. 
- The router will forward the clients broadcast DHCP messages to the remote DHCP server as unicast messages. 

#### DHCP Server configuration:
- `ip dhcp excluded-address [start ip address] [end ip address]` to exclude a range of IP addresses from being assigned by the DHCP server. This is useful for reserving IP addresses for statically configured devices such as routers, servers, etc.
- `ip dhcp pool [pool name]` to create a DHCP pool and enter DHCP pool configuration mode.
- `network [network address] [subnet mask]` to specify the network address and subnet mask for the DHCP pool. This also specifies the range of IP addresses that can be assigned to DHCP clients.
- `default-router [ip address]` to specify the default gateway for DHCP clients.
- `dns-server [ip address]` to specify the DNS server for DHCP clients.
- `lease [duration]` to specify the lease duration for DHCP clients. The duration can be specified in seconds, minutes, hours, or days. For example, `lease 7` specifies a lease duration of 7 days.
- `show ip dhcp binding` to see the current DHCP bindings on the server. This shows the IP address, MAC address, lease expiration time, and other information for each DHCP client that has been assigned an IP address by the server.

#### DHCP Relay configuration:
- `ip helper-address [ip address]` to configure a router to act as a DHCP relay agent. This command is applied on the interface that receives the clients broadcast DHCP messages. The specified IP address is the address of the remote DHCP server to which the router will forward the DHCP messages.

