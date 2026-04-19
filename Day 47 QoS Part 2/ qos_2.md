### Classification
- The purpose of QoS is to give certain kinds of network traffice priority over other during congestion. 
- Classification organizes network traffic (packets) into traffic classes (categories)
- Classification is fundamental to QoS. To give priority to certain types of traffic, you have to identiy which types of traffic to give priority to. 
- There are many methods of classifying traffic. Some example: 
 - An ACL. Traffic which is permitted by the ACL will be given certain treatment, other traffic will not. 
 - NBAR (Network Based Appplication Recognition) performs a deep packet inspection, looking beyond the layer 3 and layer 4 information to layer 7 to identiy the specific kind of traffic. 
 - In the layer 2 and layer 3 headers there are specific fields used for this purpose. 

- The PCP field of the dot1q tag in the ethernet header can be used to identiy high/low priority traffic. 
 - Only whne there is a dot1q tag. 

 - DSCP in ip header

### The IP TOS Byte
3 bits = 8 values (0-7) (this is the old format) (IPP = IP Precedence)

NEW VERSION: 1 Byte (same as before)
DSCP (6 bits) + ECN (2 bits) = 8 bits 
6 Bits = 64 values 

#### IP Precedence 
- Standard IPP markings are similar to PCP. 
 - 6 and 7 are reserver for 'network control' traffic (i.e OSPF messages between routers)
 - 5 = voice 
 - 4 = video 
 - 3 = voice signalling 
 - 0 = best effort
- Modern networks might require more value for predecedence. 

#### DSCP 
- RFC 2474 
- With IPP updated to DSCP, new standard markings had to be decided upon. 
- Common markings we should be aware about: 
 1. Default Forwarding (DF) - best effort traffic. 
 2. Expedited Forwarding (EF) - low loss/latency/jitter traffic (usually voice)
 3. Assured Forwarding (AF) - A set of 12 standard values. 
 4. Class Selector (CS) - A set of 8 standard values, provides backward compatibility with IPP. 

##### DF/EF
- DF: 
 1. DF used for best effort traffic. 
 2. The DSCP marking for DF is 0. 

- EF: 
 1. EF is used for traffic that requires low loss/latency
 2. Marking = 46 

##### AF
- AF: [YT] (https://youtu.be/4vurfhVjcMM?si=YdoNO6Qf8Rq3ihdj&t=985)
- Precedence is from highest to lowest. first 3 bits = x (class), remaining 2 bits = y (drop precedence), lsb = always 0 (god knows why)

##### CS
- This is used for backward compatibility. 
In the original IPP we have 3 bits only. 
but in order to make DSCP backwards compatible the remaining 3 bits are set to 0 and first 3 bits are for class. 

### RFC 4594 [YT] (https://youtu.be/4vurfhVjcMM?si=HiAoGAgnADBR-twQ&t=1368)

#### Queing and Congestion Management: 

ingress traffic -> routing (to interface for nat etc) -> classification (prob check DSCP for class) -> queueing  -> scheduling (weighted round robin + CBWFQ) -> transmission

the above flow is decent but the voice traffic still might have to wait, thereforce we can LLQ (Low Latency Queuing) as strict priority queues. 

but there is a drawback of LLQ, it can lead to potentially starving the round-robin queue. 

### Shaping and Policing
- Traffic shaping and policing are both used to control the rate of traffic. 

for instance a user will control the rate of traffice to 300 Mbps for example. Because the ISP has policing applied on inbound traffic to drop packets if rate is above 300 Mbps. 
