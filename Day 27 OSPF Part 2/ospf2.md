### OSPF Cost
- Reference bandwidth / interface bandwidth = cost 

- OSPF's metric is called cost.
- The OSPF cost to a destination is the total cost of teh 'outgoing/exit interfaces'
- It is automatically calculated based on the bandwidth (speed) of the interface.
- It is calculated by dividing a reference bandwidth value (100 Mbps by default) by the bandwidth of the interface.
- For example, an interface with a bandwidth of 100 Mbps would have a cost:
  Reference: 100 mbps / Interface: 10 mbps = Cost: 10
  Reference: 100 mbps / Interface: 100 mbps = Cost: 1
  Reference: 100 mbps / Interface: 1000 mbps = Cost: 1
  Reference: 100 mbps / Interface: 10000 mbps = Cost: 1

All the values less than 1 will be rounded to 1.

**Configure OSPF Cost:**

- The cost of an interface can be manually configured using the following command:
  `auto-cost reference-bandwidth [BANDWIDTH IN MBPS]`
- `ip ospf cost cost` to configure cost for a specific interface.

### OSPF Neighbors (brace for info overload!) [YT](https://www.youtube.com/watch?v=VtzfTA21ht0&t=667s)

- Once routers become neeighbors. they automatically do the work of sharing network information, calculatin routes. etc.

- When OSPF is activated on an interface, the router starts sending OSPF **hello** messages out of the interface.
- The messages are send at a regual interval (**Hello Interval**).
- The hello messages are shared to introduce the router to other potential OSPF neighbors.
- The default hello timer is 10 secs on an ethernet connection.

- Hello messages are multicast to 224.0.0.5 (multicast for all OSPF routers).
- OSPF messages are encapsulated in an IP header, with a value for 89 in the **protocol field**.

### OSPF Neighbor States:

NOTE: When we talk about states, we are talking about the state of the relationship between two neighboring routers.

1. **Down State**:

- Router sends hello BPDU out of OSPF enabled interfaces.
- The HELLO message doesn't have the router ID of the sending router.

2. **Init State**:

- When a potential neighboring router receives a Hello packet. it will add an entry for that router to its OSPF neighbor table.
- For this neighbor router's neighbor table, the relationship with the router is in the **Init state**.
- **INIT STATE** = Hello packet received, but own router ID is not in the HELLO packet.

3. **2-Way-State**:
   NOTE: At this point the two routers are already neighors.

- The 2-way state means the router has received a Hello packet with its own RID in it.
- If both routers reach the 2ways state, it means that all of the conditions have been met for them to become OSPF neighbors. They are not ready to share LSAs to build a common LSDB.
- In some network types, a DR and BDR will be elected at this point.

4. **ExStart State**:

- The two routers will now prepare to exchange information about their LSDB.
- Before, they will have to choose which one will start the exchang. They do this in the Exstart state.
- The router with higher RID will become the **Master** and initiate the exchange. The router with lower RID will become the **Slave**.
- To deceid the Master and Slave, they exchange DBD (Database Description) packets.

5. **Exchange State**:

- In this state the routers exchange DBDs which contain a list of teh LSAs in their LSDB.
- These DBDs do not include detailed information about the LSAs, just basic information.
- The routers compare the information in the DBD they received to the information in their own LSDB. To determine if they have any missing or outdated LSAs.

6. **Loading State**:

- In this state routers send Link State Request (LSR) messages to the request that their neighbors send them any LSAs they don't hav.
- LSAs are sent in Link State Update (LSU) mesasges.
- The routes send LSAck meessages to acknowledge that they received the LSAs.

7. **Full State**:

- In this state, the routers have a full OSPF adjacence and indenctical LSDBs.
- They coniinue to send and listen for HELLO packets (every 10 secs) to maintain the nighbor adjacency.
- Every time a HELLO packet is received, the 'Dead' timer (40 secs) is reset.
- If the Dead timer counts down to 0 and no Hello message is received, the neighbor is removed. 
- The routers will continue to share LSAs  as the network changes to make sure ecah router has a complete and accurate map of the network (LSDB). 

| Type | Name                               | Purpose                                                                                  |
| ---- | ---------------------------------- | ---------------------------------------------------------------------------------------- |
| 1    | Hello                              | Neighbor discovery and maintenance.                                                      |
| 2    | Database Description (DBD)         | Summary of the LSDB of the router. Used to check if the LSDB of each router is the same. |
| 3    | Link-State Request (LSR)           | Requests specific LSAs from the neighbor.                                                |
| 4    | Link-State Update (LSU)            | Sends specific LSAs to the neighbor.                                                     |
| 5    | Link-State Acknowledgement (LSAck) | Used to acknowledge that the router received a message.                                  |


## Additional Commands: 

- `show ip ospf neighbor` : shows the OSPF neighbors and their states.
- `show ip ospf interface [INTERFACE]`
- `ip ospf process-id area [AREA-ID]` : enables OSPF on an interface without using network command. In ip config mode. 
- `passive-interface default` : prevents OSPF from sending hello messages out of all interfaces. Useful for security reasons.
- `passive-interface [INTERFACE]` : prevents OSPF from sending hello messages out of a specific interface. Useful for security reasons.
- `no passive-interface [INTERFACE]` : allows OSPF to send hello messages out of a specific interface. Useful for security reasons.

