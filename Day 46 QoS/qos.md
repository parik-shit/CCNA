### IP Phones

- Traditinal phones operate over the public switched telephone network (PSTN)
- Sometimes this is called POTS (Plain Old Telephone Service)
- IP phones use VoIP technologies to enable phone calls over an IP netwrok, such as the internet.
- IP phones are connected to a switch just like any other end host.
- IP phones have an internal 3-port switch.
  1. 1 Port is the 'uplink' port that connects to the external switch
  2. 1 port is the 'downlink' port that connects to the computer.
  3. 1 port is the 'phone' port that connects to the phone itself.

- The internal switch allows the phone and the computer to share the same physical connection to the external switch.

#### Configuration

`interface <interface-name>`
`switchport mode access`
`switchport voice vlan <vlan-id>`

- This command is used to configure the interface that is connected to the IP phone as a voice VLAN interface.
- The `<interface-name>` is the name of the interface that is connected to the IP phone, and `<vlan-id>` is the VLAN ID that you want to assign to the voice traffic. This tells the switch to prioritize the voice traffic from the IP phone and to separate it from the data traffic from the computer.
  `switchport access vlan <vlan-id>` this is for the PC.

### POE

- PoE allows Power Sourcing equipment to provide power to powered device pd over an thernet cable.
- typically the PSE is a switch and PDSs are devices such as IP phones, wireless access points, security cameras, etc.
- Too much electrical current can damage electrical device.
- PoE has a process to determine if a connected device needs power, and how much power it needs.
- When a device is connected to a PoE-enabled port, the PSE (switch) sends low poer signals, monitors the response, and determines how muhc power the PD needs.
- The PSE continues to monitor the PD and supply the req amount of power (but not too much)
- Power policingb can be configured to prevent a PD from taking too much power.
  `power inline police` configures power policing with default settings. disable the port and send syslog message if a PD draws too much power.
  - equivalent to `power inline police action err-disable`
  - the interface will be put in an 'error-disabled' state and can be re-enabled with shutdown followed by no shutdown.
- `power inline police action log` does not shut down the interface if the PD draws too much power. It will restart the interface and send a syslog mesasge.

### QoS (Quality of Service)

- Voice traffic and data traffic used to use entirely separate networks.
- With the rise of VoIP, voice and data traffic are now sharing the same network.
- QoS is a set of technologies that work together to ensure the performance of critical applications (such as voice and video) by prioritizing their traffic over less critical applications (such as web browsing and file transfers).
- QoS is used to manage the following network performance parameters:
  - Bandwidth: the amount of data that can be transmitted over a network in a given amount of time.
  - Latency: the time it takes for a packet to travel from the source to the destination.
  - Jitter: the variation in latency over time.
  - Packet loss: the percentage of packets that are lost during transmission.

#### QoS - queueing
- If a network dev ice receives messages faster than it can forward them out of the appropriate interface, the messages are placed ina  queue. 
- FIFO is used by default.
- If the equeue is full, the device will drop the incoming messages until there is space in the queue again, this is called tail drop.
- Tail drop can cause problems such as global synchronization, where multiple devices drop packets at the same time, leading to a temporary decrease in network performance.
- To avoid this problem, we can use a different queueing mechanism called Weighted Random Early Detection (WRED), which randomly drops packets before the queue is full, based on the priority of the traffic. This helps to prevent global synchronization and improve overall network performance.
