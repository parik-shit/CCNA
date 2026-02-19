## EIGRP

- Enhanced interior gateway routing protocol
- Cisco proprietary protocol
- Distance vector routing protocol
- Much faster than RIP in reacting to changes in the network.
- Does not have 15 hop count limit like RIP.
- Sends messages using multicast address 224.0.0.10
- It is the only IGP that can perform _unequal_-load balancing (by default, it performs ECMP load balancing like RIP)
- METRIC = Bandwidth + Delay 
### Configuration: (EXAMPLE)

```
R1(config)#router eigrp 1 (Helped us to enable EIGRP on router and specify the AS number)
R1(config-router)#no auto-summary (Disable auto summarization of routes and enabling classless routing)
R1(config-router)#passive-interface g2/0 (Prevent EIGRP updates from a being sent out of the g2/0 interface)
R1(config-router)#network 10.0.0.0
R1(config-router)#network 172.16.1.0 0.0.0.15 (We are using wildcard mask to specify a range of IP addresses that will be included in the EIGRP process)
```

_NOTE_: The autonomous system number should be same on all routers that are running EIGRP and want to form adjacencies with each other. The AS number can be any number between 1 and 65535, but it is common practice to use numbers between 1 and 100 for private networks.

### Wildcard Mask:

- It is an inverted subnet mask

- convert the set bits in the subnet mask to 0 and the unset bits to 1.
- unset bits in the wildcard mask should match the corresponding bits in the IP address.
- set bits can vary.

Example: 172.16.1.0 0.0.0.15

- The subnet mask for this network is 11111111.11111111.11111111.11110000
- The wildcard mask is -------------- 00000000.00000000.00000000.00001111

### Commands:

- `show ip protocols`: Display information about the routing protocols that are currently running on the router, including EIGRP.
  Fields:
  1. Router ID:
     Router ID order of priority:
	1. Manual Configuration
	2. Highest IP address on a loopback interface
	3. Highest IP address on a physical interface


- `show ip protocols`: Displays information about routing protocols currently running on the router.

For EIGRP, it shows:

### 1️⃣ Router ID

Router ID priority order:

1. Manual configuration (`eigrp router-id x.x.x.x`)
2. Highest IP on loopback interface
3. Highest IP on physical interface

⚠ If you change router ID manually, you must clear the process:

```
clear ip eigrp neighbors
```

or reload.

---

### 2️⃣ AS Number

EIGRP runs using an **Autonomous System number**:

```
router eigrp 100
```

Neighbors must match the same AS number.

---

### 3️⃣ Networks Advertised

Shows which networks were enabled using the `network` command.

Example:

```
Routing for Networks:
172.16.0.0
10.0.0.0
```

This means:

* EIGRP runs on matching interfaces
* Connected routes from those interfaces are advertised

---

### 4️⃣ Passive Interfaces

Displays interfaces where:

* EIGRP runs
* But no hello packets are sent

Used for security (no neighbor formation).

---

### 5️⃣ Administrative Distance (AD)

EIGRP uses:

* 90 → Internal routes (routes learned from within an autonomous system)
* 170 → External routes

Lower AD = more preferred.

---

### 6️⃣ K Values (Metric Weights)

EIGRP metric is based on:

* Bandwidth
* Delay
* Reliability
* Load
* MTU (not used in calculation but tracked)

Default K values:

```
K1 = 1 (Bandwidth)
K3 = 1 (Delay)
Others = 0
```

If K values don’t match → neighbors will NOT form.


