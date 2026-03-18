### Multicast

It is a address that is used to send packet from one source to many hosts without making a seprate copy for each host.

#### L3 Addressing of Multicast (IP)

- Source IP in the multicast packets remain **_unicast_**.
- The **_destination_** IP represent the multicast group.

- These destination Multicast IPs belong to a special class.
  - Class D
  - All IP addresses that start with 1110
    - 224.0.0.0 - 239.255.255.255.255

- Class D is considered flat IP space
  - No concept of subnetting
  - Every single IP simply represents a multicast group.


#### L2 Addressing of Multicast (MAC)

- The goal is to have the sender and receiver agree on a single, suitable destination MAC
  - Specifically, the interested receivers/hosts should accept the frames with this destination MAC at L2. 


