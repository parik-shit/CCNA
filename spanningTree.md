# SPANNING TREE PROTOCOL (STP)

* **Standard:** IEEE 802.1D.
* **Purpose:** Prevents Layer 2 loops and broadcast storms.
* **Mechanism:** Places redundant ports in a **Blocking** state.
* **Redundancy:** Blocking ports act as backups; they transition to **Forwarding** if an active link fails.
* **BPDUs:** Interfaces in a blocking state only send/receive Bridge Protocol Data Units.

---

## WORKING OF STP

1. **BPDU Exchange:** - Switches send Hello BPDUs every **2 seconds** by default.
   - If a BPDU is received, the switch knows another switch is on that link.
2. **Root Bridge Election:** - The switch with the **Lowest Bridge ID (BID)** is elected.
   - BID = Priority + MAC Address.
3. **Designated Ports (DP):** - All ports on the Root Bridge are Designated Ports (Forwarding).
4. **Root Ports (RP):** - Every non-root switch selects **ONE** Root Port.
   - Selection is based on the **Lowest Root Path Cost**.

### STP Port Costs (Reference)
* 10 Mbps: 100
* 100 Mbps (Fa): 19
* 1 Gbps (G): 4
* 10 Gbps: 2

---

## BPDU STRUCTURE (Cisco PVST+)

> **Bridge ID (8 Bytes):**
> - **Bridge Priority (4 bits):** Increments of 4096.
> - **Extended System ID (12 bits):** Represents the VLAN ID.
> - **MAC Address (6 Bytes):** Global unique identifier.

### Operational Logic
- **Initialization:** Every switch starts by claiming to be the Root.
- **Superior BPDU:** A switch stops claiming to be Root if it receives a BPDU with a lower BID.
- **Propagation:** Once the Root is elected, only the Root originates BPDUs; others relay them.

---

## TIE-BREAKER RULES (In Order)

If path costs are equal, the following tie-breakers are used to elect ports:
1. **Lowest Root Path Cost**
2. **Lowest Sender Bridge ID** (Neighbor's BID)
3. **Lowest Sender Port Priority** (Neighbor's Port Priority - Default 128)
4. **Lowest Sender Port ID** (Neighbor's physical port number)

## Commands 

- `show spanning-tree`
- `show spanning-tree vlan [VLAN NUMBER] detail`
- `show spanning-tree interface [INTERFACE NAME] detail`

## SPANNING TREE PORT STATES 

| State      | Forwards Data | Learns MACs | BPDU Action    | Duration            |
| ---        | ---           | ---         | ---            | ---                 |
| Disabled   | No            | No          | None           | N/A                 |
| Blocking   | No            | No          | Receives Only  | Max Age (20s)       |
| Listening  | No            | No          | Sends/Receives | Forward Delay (15s) |
| Learning   | No            | Yes         | Sends/Receives | Forward Delay (15s) |
| Forwarding | Yes           | Yes         | Sends/Receives | Permanent           |

## SPANNING TREE TIMERS 

| Timer              | Default Value | Purpose                                                               |
| ---                | ---           | :-------------------------------------------------------------------: |
| Hello              | 2 Seconds     | The interval between Configuration BPDUs sent by the Root Bridge.     |
| Forward Delay      | 15 Seconds    | The time spent in EACH transition state (Listening and Learning).     |
| Max Age            | 20 Seconds    | The timeout for a stored BPDU; if not refreshed, the port resets.     |
| Message Age        | 0 to 20       | Not a fixed timer, but a counter in the BPDU that increments per hop. |
| Transmission Limit | 3 BPDUs       | Maximum number of BPDUs sent per hello interval (prevents flooding).  |

## TOOLKIT

- **Portfast** can be enabled on the ports that are connected to end hosts. 
    - It allows a ports to immediately transition to forwarding state without going through listening and learning.  
    - Should only be done to the ports that are connected to end hosts.

- **BPDU Guard** if an interface with BPDU guard enabled receives a BPDU, the interface will be shut down to prevent L2 loops. 
    - If a switch is connected to the port will get ErrDisable. 

- **BPDU Filter** is a Spanning Tree Protocol (STP) feature used to suppress the sending and receiving of BPDUs on PortFast-enabled ports. Since these ports connect to end-hosts (like PCs or printers) that do not participate in STP, sending BPDUs to them is unnecessary and wastes bandwidth.

- **Root Guard** enabled ports will enter ***broken*** (Root inconsistent) state, effectively disabling it if a superior BPDU is a received from a switch outside the network. 

- **Loop Guard** enabled ports protect the network from loops by disabling the port if it unexpectedly stops receiving BPDUs, ensuring it doesn't mistakenly enter the forwarding state. It does this by going into ***broker*** Loop inconsistent state. 
    - Loop guard should be enabled on non-designated ports (ports that are supposed to receive BPDUs)
    - Goal is to prevent a ND port from becoming DP. 

| Feature Mode            | Action on Incoming BPDU | Port Status            | Loop Safety |
| :---------------------- | :---------------------- | :--------------------- | :---------- |
| BPDU Guard              | Triggers Violation      | err-disable (Shutdown) | Highest     |
| BPDU Filter (Global)    | Removes Filter          | Normal Forwarding (STP)| Medium      |
| BPDU Filter (Interface) | Drops/Ignores BPDU      | Stays Up (No STP)      | Lowest      |

### TOOLKIT COMMANDS

| Feature      | Scope     | Command                                     | Resulting Behavior                            |
| :----------- | :-------- | :------------------------------------------ | :------------------------------------------   |
| PortFast     | Global    | `spanning-tree portfast default`            | Enables PortFast on all access ports          |
| PortFast     | Interface | `spanning-tree portfast`                    | Skips Listen/Learn; goes to Forwarding        |
| BPDU Guard   | Global    | `spanning-tree portfast bpduguard default`  | Guards all ports that have PortFast active    |
| BPDU Guard   | Interface | `spanning-tree bpduguard enable`            | Shuts down port (err-disable) if BPDU seen    |
| BPDU Filter  | Global    | `spanning-tree portfast bpdufilter default` | Stops BPDUs; resumes STP if BPDU received     |
| BPDU Filter  | Interface | `spanning-tree bpdufilter enable`           | Stops BPDUs and ignores all incoming ones     |
| Root Guard   | Interface | `spanning-tree guard root`                  | if superior BPDU is seen the port is disabled |
| Root Guard   | Global    | **DOESNT EXIST**                            | -                                             |
| Loop Guard   | Interface | `spanning-tree guard loop`                  | if superior BPDU is seen the port is disabled |
| Loop Guard   | Global    | `spanning-tree loopguard  default`          | if superior BPDU is seen the port is disabled |

## STP Configration
`stp-configuration mode?` option: `mst`, `pvst`, `rapid-pvst`
`spanning-tree vlan 1 root primary` this decreases the priority value by 4096 less than the priority of the root bridge making it the primary bridge. 
`spanning-tree vlan 1 root secondary` to configure an auxilary root bridge in case the current root bridge fails. 
**WE CAN USE THESE COMMANDS FOR LOAD BALANCING BY DOING VLAN SPECIFIC CONFIGURATION FOR ROOT BRIDGE**

## PORTFAST ON TRUNK PORTS 

When it is needed, 
- A port connected to a virtualization server with VMs in different VLAN's. 
- A port connected to a router on a stick (ROAS). 

**COMMAND** `spanning-tree portfast trunk`



