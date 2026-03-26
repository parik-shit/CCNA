### ACLs

- Access Control Lists
- Functions as a packet filter, instructing the router to permit or discard specific traffic.
- ACLs can filter traffic based on source/destination IP addresses, source/destination port numbers, and protocol types (TCP, UDP, ICMP).

#### How ACLs Work: [YT](https://youtu.be/z023_eRUtSo?si=8Lmyptu9iFzdr5I2&t=328)

- ACLs are configured _globally_ on the router.
- They are an ordered sequence of _ACEs_.
- Configuring an ACL in global config mode will not make the ACL take effect.
- The ACL must be applied to an _interface_.
- ACLs are applied either _inbound_ or _outbound_.
- When the router checks a packet against the ACL, it processes the ACEs in order, from _top to bottom_.
- If the packet is matched againt an ACE, the router takes action and stops processing the ACL.

##### Rules for ACLs:

- A maximum of once ACL cable applied to a single interface per direction (inbound or outbound).

### Implicit Deny: "What will happen if a packet doesn't match any entried in an ACL?"

- If a packet doesn't match any entry in an ACL, it will be denied by default.
- There is an "implicit deny" at the end of every ACL.

#### ACL Types:

1. _Standard ACLs_

- _Standard Numbered ACLs:_
  - COMMAND: `access-list <number> {permit|deny} <source> [wildcard mask]`
  - Standard ACls match traffice based only on the source IP address of the packet.
  - Numbered ACLs are identiefied with a number (ie. ACL 1, ACL 2, etc)
  - Standard numbered ACLs use the following range of numbers: 1-99 and 1300-1999.
  - overriding the implicit deny: `access-list 1 permit any` or `access-list 1 permit 0.0.0.0 255.255.255.255`
  - COMMAND to see ACL: `show access-lists` or `show access-lists <number>`
  - COMMAND to see standard IP ACLs: `show ip access-lists`

-* Standard Named ACLs*
  - COMMAND: `ip access-list standard <acl-name>`
  - COMMAND: `[entry number] {permit|deny} <ip> [wildcard mask]`
  - Standard ACLs match traffice based only on the source IP address of the packet. 
  - Named ACLs are identified with a name (ie. 'BLOCK_BOB')
  - Standarad named ACLs are configured by entering 'standard named ACL config mode', and then configuring each entry within that config mode. 

2. Extended ACLs

- Extended Numbered ACLs
- Extended Named ACLs

#### Applying ACLs to Interfaces:
`ip access-group number {in|out}`

