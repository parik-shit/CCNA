#### IPv6 Address Representation:

In the before slides, we learned about a more flexible way to represent IPv6.

RFC 5952 suggests standardizing IPv6.

- Leading 0s must be removed.
- ::Must be used to shorten the longest string of all-0 quartets.
- If there are two equal-length choices, for the ::, use :: to shorten the one on the _left_.
- Hexadecimal digits must be in _lowercase_.

### IPv6 Header: [YT](https://youtu.be/rwkHfsWQwy8?si=W15B6yjGzdLBpk3E&t=359)

| Field Name       | Size (bits)     | Description                                  |
| ---------------- | --------------- | -------------------------------------------- |
| Version          | 4               | IP version (always 6)                        |
| Traffic Class    | 8               | Similar to QoS / DSCP                        |
| Flow Label       | 20              | Identifies packet flow for special handling  |
| Payload Length   | 16              | Size of payload (excluding header)           |
| Next Header      | 8               | Type of next header (TCP, UDP, ICMPv6, etc.) |
| Hop Limit        | 8               | Like TTL in IPv4                             |
| Source Address   | 128             | Sender IPv6 address                          |
| Destination Addr | 128             | Receiver IPv6 address                        |
|                  | Total: 40 Bytes |                                              |

---

### Solicited-Node Multicast Address:

- An IPv6 solicited-node multicast address is calculated from a unicast address.
- Example:
  **ff02:0000:0000:0000:0000:0001:ff** + Last 6 hex digits of unicast address

### Neighbor Discovery Protocol (NDP): [YT](https://youtu.be/rwkHfsWQwy8?si=MfaiKhF0akCLq64D&t=810)

- It is a protocoal used with IPv6.
- Function:
  1. **Address resolution** (IPv6's replacement for ARP)
  - Two message types are used:
    1. Neighbor Solicitation (_NS_) = ICMPv6 Type 135 (equivalent to ARP _request_)
    2. Neighbor Advertisement (_NA_) = ICMPv6 Type 136 (equivalent to ARP _reply_)
  2. **Router discovery** (IPv6's replacement for ICMP Router Discovery)
  - Two message types are used:
    1. Router Solicitation (_RS_) = ICMPv6 Type 133 (equivalent to ICMP Router Discovery _request_)(send to FF02::2)
    2. Router Advertisement (_RA_) = ICMPv6 Type 134 (equivalent to ICMP Router Discovery _reply_)(send to FF02::1)
  3. **Prefix discovery** (IPv6's replacement for DHCPv6)
  4. **Parameter discovery** (IPv6's replacement for DHCPv6)

_NOTE: The journey between one router to another is known as a "hop".\*\*
NOTE: ICMP gives a traceroute which also shows time taken for each hop._

### SLAAC:

- SLAAC stands for Stateless Address Autoconfiguration.
- Hosts use the RS/RA messages to learn the IPv6 prefix of the local link (ie. 2001:db8::/64), and then automatically generate an IPv6 address.
- Using the `ipv6 address prefix/prefix-length eui-64` command, you need to manually enter the prefix.
- using the `ipv6 address autoconfig` command, you don't need to enter the prefix. The device uses NDP to learn the prefix used oni the local link.

### Duplicate address Detection (DAD):

- It allows hosts to check if other devices on the local link are using the same IPv6 address.
- Any time an IPv6-eanbled interface initializes (no shutdown command), or an IPv6 address is configured on an interface (by any method: manual, SLAAC, etc), it performs DAD.
- DAD uses two messages: _NS_ and _NA_.
- The host will send an NS to its own IPv6 address. If it doesn't get a reply, it knows the address is unique.

---

### IPV6 Static Routing [YT](https://youtu.be/rwkHfsWQwy8?si=0UwobnMgx6vkIx2Y&t=1526)

_NOTE: if IPv6 routing is disabled, the router will be able to send and receive packets, but will not route them._
_NOTE: Exit interface has to be specified for a link-local next-hop_

- A connected network route is automatically added for each connected network.
- A local host route is automatically added for each address configured on the router.
- Routes for link-local addresses are not added to the routing table.

COMMAND: `ipv6 route destination/prefix-length {next-hop-address | exit-interface [next-hope]} [administrative-distance]`

- Type of routes:
  1. Network route
  2. Host route: \OO/ cant think of one
  3. Default route: `ipv6 route ::/0 {next-hop-address | exit-interface [next-hope]} [administrative-distance]`
