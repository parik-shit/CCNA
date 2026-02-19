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

1.  Network: [Example of configuring RIPv2](https://youtu.be/N8PiZDld6Zc?si=INA9I8JqunOE_BsB&t=766)
    - The `network` command is used to specify which networks will be advertised by RIP.
    - activate RIP on an interface by including the network address of the interface in the `network` command.
    - Form adjacencies with other RIP routers that have interfaces in the same network.

2.  Passive Interface:
    - The `passive-interface` command is used to prevent RIP updates from being sent out of a specific interface.
    - This is useful for interfaces that are connected to end devices (like PCs) or for security reasons.
    - The network prefix of the interface is still shared.

3.  Default infomation originate
    - The `default-information originate` command is used to advertise a default route into the RIP routing protocol.
    - Helps other routers in the network to know about the default route.

4.  show ip protocols:
    - The `show ip protocols` command is used to display information about the routing protocols that are currently running on the router.
    - It shows which routing protocols are enabled, their configuration, and their status.

5.  maximum-paths:
    - The `maximum-paths [NUMBER of PATHS]` command is used to specify the maximum number of equal-cost paths that can be installed in the routing table for a single destination.
    - This allows for load balancing across multiple paths to the same destination.

6.  distance:
    - The `distance [ADMINISTRATIVE DISTANCE]` command is used to set the administrative distance for RIP routes.
    - This can be useful if you want to prefer RIP routes over other routing protocols or vice.

7. no auto summary: 
    - The `no auto-summary` command is used to disable automatic summarization of routes in RIP.
    - By default, RIP summarizes routes at classful boundaries. This command allows for more specific route advertisements, which is important when using VLSM or CIDR.
