### Simple Network Management Protocol (SNMP)

- Uses UDP port 161 for SNMP messages and UDP port 162 for SNMP trap messages.

- RFC 1065
- RFC 1066
- RFC 1067
- Above RFC make up SNMPv1

- There are two main types of devices in SNMP:

1.  Managed Devices:

- These are teh devices being managed using SNMP.
- For example, network devices like routers and switches.

2.  Network Management Systems (NMS):

- These are the systems that monitor and manage the managed devices.
- This is the SNMP "server".

### SNMP Operations

- Three main operations in SNMP:

1. Managed devices can notify the NMS of events (e.g., link down, high CPU usage).
2. NMS can query managed devices for information (e.g., get the current CPU usage
3. NMS can tell the managed devices to change aspects of their configuration.

### SNMP Components

- SNMP manager
- SNMP application
- SNMP agent
- Management information base (MIB): is the structure that contains the variables that are managed by SNMP.
- Each variable is identified with an Object ID (OID), which is a unique identifier for that variable.
- Example variables: interface status, traffic throughput, CPU usage, temerature, etc.
- SNMP object IDs are organized in a hierarchial strcuture.
- Example: .1.3.6.1.2.1.1.5 (here the NMS can query the device for its name)

### SNPMP Versions

- SNMPv1: The original version of SNMP.
- SNMPv2c:
- Allows the NMS to retrieve large amounts of information in a single request, so it is more efficient.
- 'c' refers to the 'community strings' used as passwords in SNMPv1, removed from SNMPv2, and then added back for SNMPv2c.
- SNMPv3:
- A much more secure version of SNMP that supports strong encryption and authentication. Whenever possble, this version should be used.

### SNMP Messages

| Message Class | Description                                                                 | Messages              |
| ------------- | --------------------------------------------------------------------------- | --------------------- |
| Read          | Messages sent by the NMS to read information from the managed devices       | Get, GetNext, GetBulk |
|               | (e.g., “What’s your current CPU usage?”)                                    |                       |
| Write         | Messages sent by the NMS to change information on the managed devices       | Set                   |
|               | (e.g., change an IP address)                                                |                       |
| Notification  | Messages sent by the managed devices to alert the NMS of a particular event | Trap, Inform          |
|               | (e.g., interface going down)                                                |                       |
| Response      | Messages sent in response to a previous message/request                     | Response              |

- GET: A request sent from the manager to the agent to retrieve the value of a variable (OID), or multiple variables. The agent will send a Response message with teh current valuye of each variable. 

