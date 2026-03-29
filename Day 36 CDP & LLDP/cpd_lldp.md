### Layer 2 Discovery Protocols
- Layer 2 discovery protocols such as CDP and LLDP share information with and discover information aboout neighboring (connected) devices. 
- The shared information includes host name, IP address, device type, etc. 
- CDP is a Cisco proprietary protocol. 
- LLDP is an industry standard protocl (*IEE 802.1AB*)
- Because they share information about the devices in the network, they can be considered a security risk and are often not used. It is up to the network engineer/admin to decide if they want to use them in the network or not. 

### CDP
- Cisco Proprietary
- It is enabled on Cisco devices (routers, switches, firewalls, ip phones) by default. 
- CDP messages are periodically sent to multicast MAC address *0100.0CCC.CCCC*
- COMMAND: `show cdp`
- COMMAND: `show cdp interface` // check the interfaces CDP is enabled on 
- COMMAND: `show cdb neighbors` // to see the CDP neighbors 
- COMMAND: `show dbp neighbors detail`
- COMMAND: `show cdp entry [name]`

#### CDP configuration commands: 
- `[no] cdp run` enable/diable globally
- `[no] cdp enable` enable/disable on a specific interface
- `cdp timer [seconds]` configure the timer 
- `cdp holtime [second]` configure hold time (*default 180 secs*)
- `[no] cdp advertise-v2` enable/disable cdpv2

--- 

### Link Layer Discovery Protocol (LLDP): 
- LLDP is an industry standard protocl (IEEE 802.1AB)
- It is usually disabled on Cisco devices by default, so it must be manually enabled. 
- A device can run CDP and LLDP at the same time. 
- LLDP mesasges are periodically sent to multicast MAC address 0180.C200.000E. 
- When a device receives an LLDP message, it processes and discards the message. It does NOT forward it to other devices. 
- LLDP messages are sent every *30 secs* by default.
- LLDP has an additional timer called the 'einitialization delay'. If LLDP is enabled (globally or an interface),, this timer will delay the actual initialization of LLDP. 2 Secs by default. (it also has something to do with mac address flapping)

#### LLDP configuration commands: 
- `lldp run` to enabled lldp globally
- `lldp transmit` to enable LLDP on specific interface (tx)
- `lldp receive` to enable LLDP on specific interface (rx)
- `lldp timer [seconds]`
- `lldp holdtime [seconds]` configure the LLDP holdtime. 






