## Ether Channel

EtherChannel groups multiple interfaces to work as a single interface.

- STP treats this group as a single interface.
- Some other names of etherChannel are:
    1. Port Channel
    2. LAG (Link Aggregation Group)

### Problem Ether Channels solve:

- Oversubscription: When the bandwidth of the interfaces connected to endhosts is greater than the bandwidth of the connection to the distribution switches,this is called **oversubscription**.

### How EtherChannel Load Balances:

- EtherChannel load balances based on 'flows'.
- A flow is a communication between two nodes in the network.
- Frames in the same flow will be forwarded using the same physical interface.
- If frames in the same flow were forwarded using different physical interfaces then the order could be different, leading to issues.

### Interface Selection Calculation:

- We can change the inputs used for interface selection.
- Inputs that can be used:
    1. **Source MAC**
    2. **Destination MAC**
    3. **Source and Destination MAC**
    4. **Source IP**
    5. **Destination IP**
    6. **Source and Destination IP**

### COMMANDS:

- `show etherchannel load-balance`
- `port-channel load-balance [INPUT]` Used to change the input for selection.

### Type of configuration:

  1. **PAGP** (cisco's proprietary)
  2. **LACP** (IEEE)
  3. **Static EtherChannel**

#### Configuration

- Member interfaces need to have same configuration.

- **STEP 1** Select a range of interface to be configured
  COMMAND: `interface range [RANGE]`

- **STEP 2** SELECT configuration mode
  COMMAND: `channel-group [NUMBER] mode [MODE]`
  OPTIONS:
    1. active [LACP]
    2. auto [PAGP]
    3. desirable [PAGP]
    4. on [EtherChannel only]
    5. passive [LACP]

  Rule of thumb:
    - auto + auto = no EtherChannel
    - desirable + auto = EtherChannel
    - desirable + desirable = EtherChannel

    - passive + passive = no EtherChannel
    - active + passive = EtherChannel
    - active + active = EtherChannel

    - on + on = EtherChannel (static EtherChannel needs both interface mode 'on') 

  **BONUS**: We can check the channel that we created in interface brief.

### Configuring Layer 3 EtherChannel: 

- **STEP 1** set the **interface range** as no switch port to make them operate at layer 3
  - `no switchport`

- **STEP 2** give an IP to the port channel 
  - `int [CHANNEL]`
  - `ip address [IP] [SUBNET MASK]`

### Rule of a port-channel

All bundled interfaces must match: 

  1. Speed
  2. Duplex
  3. VLANs
  4. Trunking Mode
  5. Native VLAN 
  6. Allowed VLAN list 


## **Memory Shortcut**

`channel-group [GROUP] mode [MODE]`
`port-channel load-balance ?`
`show etherchannel summary`
`show etherchannel port-channel [GROUP]`
