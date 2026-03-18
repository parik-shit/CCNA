### First Hop Redundancy Protocols (FHRP)

- FHRP is a family of protocols that provide redundancy for the default gateway in a network.
- The main purpose of FHRP is to ensure that if the primary default gateway fails, a backup gateway can take over without any disruption to network traffic.

### Working:

- A virtual IP is configured on the two routers , and a virtual MAC is generated for the virtual IP( each FHRP uses a different format for the virtual MAC )
- An active router and a stadby router are elected. (different FHRPs use different terms)
- End shosts in the network are configured to use the virtual IP as their default gateway.
- The active router replies to ARP requests using the virtual MAC address, so traffic destined for other networks will be sent to it.
- If the active router fails, the standby becomes the next active router. The new active router will send gratuitous ARP messages so that switches will updated their MAC address tables. It now functions as the default gateway.
- If the old router recovers, it wont take over the active role until the next failover event.

### HSRP (Hot Standby Router Protocol):

- Cisco proprietary protocol.
- An active and standby router are elected.
- There are two versions: version 1 and version 2.
  Version 2 adds IPv6 support and increase the number of groups that can be configured.
- Multicast IPv4 address: v1 = 224.0.0.2, v2 = 224.0.0.102
- Virtual MAC address: v1 = 0000.0c07.acxx, v2 = 0000.0c9f.fxxx (xx is the group number in hex)
- In a situation with multiple subnets/VLANs, you can configure a different active router in each subnet/VLAN to load balance.

- Configurtion:
  `standby [group-number] ip [virtual-ip-address]` to configure the virtual IP address and group number on an interface.
  `standby [group-number] priority [priority-value]` to set the priority of a router (the higher the value, the more likely it is to become active).
  `standby [group-number] preempt` to allow a router to take over the active role if it has a higher priority than the current active router.
  `show standby` to display the status of HSRP groups and their members.
 

### VRRP (Virtual Router Redundancy Protocol):

- Open Standard
- A master and backup router are elected.
- Multicast IPv4 address: 224.0.0.18
- Virtual MAC address: 0000.5e00.01xx (xx is the group number in hex)

### GLBP (Gateway Load Balancing Protocol):

- Cisco proprietary protocol.
- Load balances among multiple routers within a single subnet.
- An AVG (Active Virtual Gateway) is elected.
- Up to 4 AVFs (Active Virtual Forwarders) are assigned by the AVG ( the avg itself can be an AVF, too )
- Each AVF acts as teh default agteway for a portion of the hosts in the subnet.
- Multicast IPv4 address: 224.0.0.102

### Comparison of FHRP Protocols:

| FHRP | Terminology      | Multicast IP                  | Virtual MAC                           | Cisco proprietary? |
| ---- | ---------------- | ----------------------------- | ------------------------------------- | ------------------ |
| HSRP | Active / Standby | v1: 224.0.0.2 v2: 224.0.0.102 | v1: 0000.0c07.acXX v2: 0000.0c9f.fXXX | Yes                |
| VRRP | Master / Backup  | 224.0.0.18                    | 0000.5e00.01XX                        | No                 |
| GLBP | AVG / AVF        | 224.0.0.102                   | 0007.b400.XXYY                        | Yes                |
