## Dynamic Routing

- Dynamic routing helps routers to learn about routes without manually configuring static routes.

- Route is represented with **O** in the routing table.

### Types of Dynamic Routing Protocols:

- **IGP** (Interior Gateway Protocol): Used to share routes in between a single autonomous system (i.e a company).
- **EGP** (Exterior Gateway Protocol): Used to share routes between different autonomous system.

| Property      | IGP                                 | EGP                  |
| ------------- | ----------------------------------- | -------------------- |
| **Algorithm** | 1. Distance Vector (RIP and EIGRP)  | 1. Path Vector (BGP) |
|               | 2. Link State (OSPF(imp) and IS-IS) |                      |

### Distance Vector Protocols: (Router already knows the metric from the advertisements)

- Distance vector protocols were invented before link state protocols.
- Distance vector protocls operate by sending the following to their directly connected neighbors:
  - their known destination network
  - ther metric to reach their known desitnation network

- This method of sharing route information is often called 'routing by rumor'
- This is because the router doesn't know about the network beyond its neighbors. It only knows the information that its neighbors tell it.
- Called 'distance vector' because the routers only learn the 'distance' (metric) and 'vector' (direction, the next-hop router) of each route.

### Link State Protocols: (Router has to calculate the metric from the map)

- In **link state** routing protocol, every router creates a 'connectivity map'.
- To allow this, each router advertises information about its interface (connected network) to its neighbors. These advertisements are passed along to other routers, until all routers in the network develop the same map of the network.
- Each router independently uses this map to calculate the best routes to each destination.
- Link state protocols use more resources (CPU) on the router, because more information is shared.
- However, link state protocols tend to be faster in reacting to changes in the network than distance vector protocols.

### Dynamic Routing Protocol Metrics:

- A router's route table contains the best route to each destination netwrok it knows about.
- If a router using a dynamic routing protocol learns two different routes to the same destination, how does it determine which is 'best'?
- It uses the **metric** value of the routes to determine which is best. A lower metric = beter.

- If two routes have equal metric then, both the routes will be added to the routing table. Traffic will be loadbalanced accross the routes (**ECMP** (Equal Cost Multi-Path)).
- ECMP can also be implemented on static routes (there are a couple of nuances though)

| **IGP** | **Metric**                                   | **Explanation**                                                                                                                                                           |
| ------- | -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RIP     | Hop Count                                    | Each router in the path counts as one 'hop'. The total metric is the total number of hops to the destination. **Link of all speeds are equal.**                           |
| EIGRP   | Metric based on bandwidth & delay by default | Complex formula that can take into account many values. By default, the bandwidth of the slowest link in the route and the total delay of all links in the route are used |
| OSPF    | Cost                                         | The cost of each link is calculate based on bandwidth. The total metric is the total cost of ecah link in the route.                                                      |
| IS-IS   | Cost                                         | The total metric is the total cost of each link in the route. The cost of each link is not automatically calculated by default. All links have a cost of 10 by default.   |

### Administrative Distance:

- The administrative distance is a value that is used to determine the trustworthiness of a route.

| **Route Type** | **Administrative Distance** |
| -------------- | --------------------------- |
| Connected      | 0                           |
| Static         | 1                           |
| EIGRP          | 90                          |
| OSPF           | 110                         |
| IS-IS          | 115                         |
| RIP            | 120                         |
| External BGP   | 20                          |
| Internal BGP   | 200                         |
| Unknown        | 255                         |

### Floating Static Routes:

- A floating static route is a static route that has a higher administrative distance than the dynamic routes.
- This allows the static route to be used as a backup route in case the dynamic route fails
- The route wil be inactive (not in the routing table) as long as the dynamic route is active. If the dynamic route fails, the static route will become active and be used to forward traffic to the destination network.
