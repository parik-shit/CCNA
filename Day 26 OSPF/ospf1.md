## OSPF

- It is a Link State Routing Protocol (check dynamicRouting.md for more details about link state protocols)

- It uses **Shortest Path First** algo made by Dijsktra.

- Three versions:
  1. OSPFv1 (old, not in use)
  2. OSPFv2 (for IPv4)
  3. OSPFv3 (for IPv6)

- The routers store information in ***Link State Advertisements (LSAs)***, which are used to build a map of the network.
- LSA are organized in a structure called  **Link State Database (LSDB)**.  

- Routers will flood LSAs until all routers in the network have the same LSDB. 

- **Mental Model: A router generates LSA and all routers in a network share the same LSDB. Inside the LDSB there are LSA.**

- LSA has an aging timer of 30 mins. LSA will be flooded again after the time expires. 


### OSPF Areas: [YT](https://www.youtube.com/watch?v=pvuaoJ9YzoI&t=560s)
- OSPF supports the concept of areas, which are logical groupings of routers within an OSPF network.
34- Each area has its own LSDB, and routers within the same area share the same LSDB.
- Areas are used to reduce the size of the LSDB and to improve the scalability of the OSPF network.
- The backbone area (Area 0) is a special area that all other areas must connect to. It is used to exchange routing information between different areas.
- Routers with all interfaces in the same area are called **Internal Routers**.
- Routers with interfaces in multiple areas are called **Area Border Routers (ABR)**.
- Routers connected to the backbone area (area 0) are called **Backbone Routers**.
- Routers that connect to other OSPF networks (like other AS) are called **Autonomous System Boundary Routers (ASBR)**.
- An *intra*-area route is a route to a destination inside the same OSPF area. 
- An *inter*-area route is a route to a destination in a different OSPF area. 

- Additional Rules: 
   - All the areas should be contigious 
   - All the areas should have atleast one ABR (except area 0 ). 
   - OSPF interfaces in the same subnet must be in the same area. 


### OSPF Configuration: 

### Router ID: 
- OSPF uses a 32-bit Router ID to identify each router in the OSPF network.

Router ID priority order:
1. Manual Configuration 
2. Highest IP address on a loopback interface 
3. Highest IP address on a  physical interface 

in order to mannually configure the router ID, use the following command: 
`router-id x.x.x.x`
after this we need to clear the process.

we can also change the maximum number of paths using,
`maximum-paths [NUMBER of PATHS]`

