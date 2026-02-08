## RIP

- Routing information Protocol
- Distance Vector Protocol
- Uses hop count as its metric (max 15 hops)
- Has 3 versions: RIPv1, RIPv2, RIPng
- Uses two message types: Request and Response
- by default routing tables are shared every 30 seconds

- _RIPv1_ is classful (doesn't support VLSM or CIDR)
- RIPv1 doesn't include subnet mask information in its advertisements.
- _RIPv2_ is classless (supports VLSM and CIDR)
- network prefixes are included in RIPv2 advertisements, so subnet mask information is included.
- _RIPng_ is the version of RIP for IPv6

### Configuration:

- To enable RIP on a router, use the following commands:

```
router rip
version 2
network <network-address>
```

- When we are configuring the network it is classful, so all the IP that fall in the range will be included.
- The advertisments will have network prefix though.

## Commands:

1. Network: [Example of configuring RIPv2](https://youtu.be/N8PiZDld6Zc?si=INA9I8JqunOE_BsB&t=766)

  - The `network` command is used to specify which networks will be advertised by RIP.
  - activate RIP on an interface by including the network address of the interface in the `network` command.
  - Form adjacencies with other RIP routers that have interfaces in the same network.

2. Passive Interface: 

  - The `passive-interface` command is used to prevent RIP updates from being sent out of a specific interface.
  - This is useful for interfaces that are connected to end devices (like PCs) or for security reasons.
  - The network prefix of the interface is still shared. 
