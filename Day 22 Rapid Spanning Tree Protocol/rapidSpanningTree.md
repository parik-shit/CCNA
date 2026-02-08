| Industry Standard (IEEE)        | Cisco versions                 |
| ------------------------------- | ------------------------------ |
| spanning tree protocol (802.1D) | Per VLAN spanning tree         |
| Original STP                    | Cisco's upgrade to (802.1D)    |
| One instance for all VLANS      | Each VLAN has its own instance |
| Can't be load balanced          | Can be load balanced           |

| Rapid Spanning Tree protocol (802.1w)                  | Cisco versions (RVSTP+)        |
| ------------------------------------------------------ | ------------------------------ |
| Much faster at adapting to network changes than 802.1D | Cisco's upgrade to 802.1w      |
| Once instance for all VLANs                            | Each VLAN has its own instance |
| Can't be load balanced                                 | Can be load balanced           |

| Multiple Spanning Tree protocol (802.1s)           |
| -------------------------------------------------- |
| Uses modified RSTP mechanics                       |
| Can group multiple VLANs into difference instances |
| Can be load balanced                               |

Cisco's Summary:
RSTP is not a timer-based STP like 802.1D.
Therefore, RSTP offers an improvement over the 30 sec or more than 802.1D takes to move a link to forwarding.
The heart of the protocol is a new bridge-bridge handshake mechanism, which allows port move directly to forwarding.

| *State*      | Forwards Data | Learns MACs | BPDU Action    | Stable/Transitional |
| ---------- | ------------- | ----------- | -------------- | ------------------- |
| Discarded  | No            | No          | No/Yes         | Stable              |
| Learning   | No            | Yes         | Sends/Receives | Transitional        |
| Forwarding | Yes           | Yes         | Sends/Receives | Stable              |

- if a port is adminitratively disabled = Discarding State
- if a port is blocking traffic to prevent layer 2 loops = Discarding State

## RSTP Port Roles: 

- The **root port** remains unchanged in RSTP (just like classic STP)

- The **designated port** also remains unchanged. 

- The **non-designated port** role is split into two seprate roles in RSTP: 
  1. **Alternate Port**  
    - It is a discarding port that receives superior BPDU from another switch. 
    - If the best root port fails it immediately switches the next best port to forwarding state. 
    
  2. **Backup port** :  
    - The redundant port that is connected to a hub is the backup port. 

## RSTP Features: 

1. *Uplinkfast* (optional)
  - It is used to deal with direct link failures.

2. *Backbonefast* (optional)
  - It is used to deal with indirect link failures. 

## COMMANDS

- `spanning-tree mode rapid-pvst`: to switch to rapid pvst. 

## **RSTP WORKING**

- All switches running rapid STP send their own BPDUs every Hello time (2 secs). 
- Switches age the BPDU info much more quickly. If the switch missed 3 BPDU (6 secs) from a neighboring switch, the neigbor is considered lost, all the mac addresses learned on that interface are flushed. 

## RSTP Link Types

1. Edge: Ports that are connected to end hosts. 
  - COMMAND: `spanning-tree portfast`

2. Point-Point: Ports that connect 2 swiches. 
  - COMMAND: `spanning-tree link-type point-to-point`

3. Shared: Ports on a connection hub. These must operate in half duplex.  
  - COMMAND: `spanning-tree link-type link` **We don't need to configure it manually** 

