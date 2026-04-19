#### Private IPv4 Address

- IPv4 doesn't provide enugh addresses for all devices that need an IP address in the modern world.
- The long-term solution is to switch to IPv6.

- There are 3 main short-term solutions to this problem:
  1. Network Address Translation (NAT)
  2. Private IPv4 addresses 
  3. Classless Inter-Domain Routing (CIDR)

- RFC 1918 specifies the following IPv4 address ranges as private: 
 - 10.0.0.0/8 (10.0.0.0 to 10.255.255.255)
 - 172.16.0.0/12 (172.16.0.0 to 172.31.255.255)
 - 192.168.0.0/16 (192.168.0.0 to 192.168.255.255) 

- You are free to use these addresses in your networks. They don't have to be globally unique. (Can't be used on the public internet)

### NAT
- Network Address Translation (NAT) is used to modify the source and /or destination IP addresses of packets. 
- There are various reasons to use NAT, but the most common reason is to allow hosts with private IP addresses communicate with other hosts over the interent. 
- For CCNA just learn source NAT

### Static NAT
- It involves statically configuring 1-to-1 mappings of private IP addresses to public IP addresses. 
- An inside local IP address is mapped to an inside global IP address. 
 - Inside local = The IP address of the inside host, from the perspective of the local network. 
 - Inside global = The IP address of the inside host, from the perspective of the global network.

#### Static NAT configuration example: [YT]*(https://youtu.be/2TZCfTgopeg?si=uAAfV8Jwk-UM-Z2k&t=981)
Commands: 
`ip nat inside` in interface configuration mode to specify that the interface is on the inside of the network.
`ip nat outside` in interface configuration mode to specify that the interface is on the outside of the network.
`ip nat inside source static <inside local IP> <inside global IP>` in global configuration mode 
`show ip nat translations` to verify the NAT translations.
`clear ip nat translation *` to clear the NAT translations. (It will clear only the dynamic translations, not the static ones)
`show ip nat statistics` to view NAT statistics.
