NOTE: The first 3 quartets represent "Global Routing Prefix"

### Configuring IPv6 addresses (EUI-64) 
- EUI stands for Extended Unique Identifier. 
- It is used to convert the 48 Bit MAC to host portioon of an IPv6 address. 

### STEPS: 
1. Divide the MAC into two 24 bit halves.
2. Insert FFFE in between the halves. 
3. Invert the 7th Bit. 

### Configuration: 
`ipv6 address [NETWORK PREFIX] eui-64` (in interface configuration mode)

#### Why invert the 7th bit?

- Mac addresses can be divided into two types:
  1. UAA (universally adminitered address): Uniquely assigned to the device by the manufacturer. 
  2. LAA (locally adminitered address): Manually assigned by an admin (with the mac-address command on the interface) or protocol. Doesn't have to be globally unique.

- You can identify a UAA or LAA by the 7th bit of the MAC address, called the U/L bit (universal local bit): 
  - U/L is set = UAA
  - U/L is not set = LAA 

- In LAA opposite of UAA is followed. 

### Global Unicast address: 
- Global unicast ipv6 addresses are public addresses which can be used over the internet. 
- Must register to use them. Because they are public addresses, it is expected that they are globally unique. 
- Originally defined as the **2000::/3** block (2000:: to 3fff:ffff:ffff:ffff:ffff:ffff:ffff:ffff)
- First 3 Bits are fixed. 

### Unique Local Addresses: 
- Unique local IPv6 addresses are private addresses which cannot be used over the interenet. 
- You do not need to register to use them. They can be used freely within internal networks and don't need to be globally unique. Can't be router over the internet.
- First 7 bits are fixed. Uses the address bloack *FC00::/7* (FC00:: FDFF:FFFF:FFFF:FFFF:FFFF:FFFF) (For some reason 8th bit is also set but don't ask why)

### Link Local Addresses: 
- Link-local IPv6 addresses are automatically generated on IPv6-enabled interfaces. 

- Link-local means that these addresses are used for communication within a single link (subnet). Routers will not route packets with a link-local destination IPv6 address. 
- Common uses of link-local addresses: 
  - routing protocol peerings (OSPFv3 uses link-local addresses for neighbor adjacencies)
  - Next-hop addresses for static routes. 
  - Neighbor Discovery Protocol (NDP, Ipv6's replacemenet for ARP) uses link-local addresses to function. 
- First 10 Bits are fixed. Uses the address block *FE80::/10* (FE80:: to FEBF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF) (For some reason 11th bit is also set but don't ask why)

### Multicast Addreses: 
- One-to-Many addresse (only for the hosts that have joined the multicast group)
- Rerserve the address block *FF00::/8* for multicast addresses.
- IPv6 multicast address start from FFF02::[]  (similar to IPv4 where the pattern for the first 4 bits is 1110 )
- IPv6 doesn't use broadcast addresses. Instead, it uses multicast addresses to achieve the same functionality.
  - FF02::1 is the all-nodes multicast address (equivalent to IPv4's broadcast address). All IPv6-enabled devices must join this multicast group.
  - FF02::2 is the all-routers multicast address. All IPv6-enabled routers must join this multicast group.

#### IPv6 multicast scopes: 
- IPv6 defines multiple multicast 'scopes' which indicate how far the packet should be forwarded. 
- The addresses in the previous slide all use the 'link-local' scope (FF02), which stays in the local subnet. 
- *Interface-local* (FF01): Packet stays within the device (used by netorking stack to communicate with application)
- **Link-local (FF02)**
- *Site-Local* (FF05): The packet can be forwarded by routers. Should be limited to a single physical location (not forwarded over a WAN)
- *Organization-local* (FF08): Wider in scope than site-local (an entire company/organization). 
- *Global* (FF0E): No boundaries. Possible to be routed over the internet. 

### Anycast addresses: (Multiple possible destination but traffice is sent to one)
- A new feature of IPv6 
- Anycast is 'one-to-one-of-many'
- Many routers are configured with same IPv6 address. 
  - They use a routing protocol to advertise the address. 
  - When hosts sends packlets to that destination address, routers will forward it to the nearest router configured with that IP (based on the routing metric). 

- There is no specific address range for anycast addresses. Use a regular unicast address (global unicast, unique local) and specify it as an anycast address

#### Other IPv6 Addresses: 
- :: The unspecified IPv6 address. 
- ::1 The loopback address: Used to test the protocol stack on the local device. (IPv4 equiv: 127.0.0.0/8) 
