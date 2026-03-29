#### Advantages of named ACL config mode 
- You can easily delete ASE with `no [sequence-number]`
- You can insert new enteries in between by specifying entry number. 

#### Resequencing ACLs 
- There is a resequencing functino that helps edist ACLs. 
- The command is `ip access-list resequence acl-id starting-seq-num increment`

### Extended ACLs: 
- Extended ACLs function mostly the same as standard ACLs. 
- They can be numbered or named, just like standard ACLs
  - Numbered ACLs use the following ranges: 100 - 199, 2000 - 2699
- They are processed from top to bottom, just like standard ACLs. 
- However, they can match traffic based on more parameteres, so they are more precise (and more complex) than standard ACLs. 
- We will focus on matching based on these main parameters: *Layer 4 protocol/port* , *source address*, and *destination address*. 

- COMMAND:  `access-list <number> {permit|deny} <protocol> <source> [wildcard mask] <destination> [wildcard mask]`

- COMMAND: `ip access-list extended <acl-name>` (enter extended named ACL config mode)
  - COMMAND: `[entry number] {permit|deny} <protocol> <source> <destination>` (configure ACL entry in extended named ACL config mode)

#### Few common scenerios for extended ACLs:
1. Allow all trafic: `permit ip any any`
2. Prevent 10.0.0.0/16 from sending UDP traffic to 192.168.1.1/32: `deny udp 10.0.0.0 0.0.255.255 192.168.1.1`
3. Prevent 172.16.1.1/32 from pinging hosts in 192.168.0.0/24: `deny icmp host 172.16.1.1 192.168.0.0 0.0.0.255`

### Matching the TCP/UDP port number: 
- When matching TCP/UDP, you can optionally specify the source and/or destinatino port numbers to match. 

`access-list <ACL_NUMBER> {permit | deny} {tcp | udp} \
<SRC_IP> <SRC_WILDCARD> [OPERATOR <SRC_PORT>] \
<DEST_IP> <DEST_WILDCARD> [OPERATOR <DEST_PORT>]`

| Operator | Meaning                  |
| -------- | ------------------------ |
| `eq`     | Equal to a specific port |
| `neq`    | Not equal to             |
| `gt`     | Greater than             |
| `lt`     | Less than                |
| `range`  | Between two ports        |

#### Few common senarios for matching TCP/UDP port numbers:
1. Allow traffic from 10.0.0.16 to access the server at 2.2.2.2/32 using HTTPS. 
`permit tcp 10.0.0.0 0.0.255.255 host 2.2.2.2 eq 443`

2. Prevent all hosts using source UDP port numbers from 20000 to 30000 from accessing the server at 3.3.3.3/32. 
`deny udp any range 20000 30000 host 3.3.3.3`

3. Allow hosts in 172.16.1.0/24 using a TCP source port greater than 9999 to access all TCP ports on server 4.4.4.4/32 except port 23. 
`permit tcp 172.16.1.0 0.0.0.255 gt 9999 host 4.4.4.4 neq 23`
