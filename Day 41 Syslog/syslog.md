### Syslog 
- Syslog is an industry standard protocol for message logging. 
- On network devices, Syslog can be used to log events such as changes in interface status, changes in OSPF enighbor status, system restarts, etc. 
- The messages can be displayed in the CLI, saved in the devce's RAM, or sent to an external Syslog server.
- Logs are essesntial when troubleshooting network issues, as they can provide insight into what is happening on the device.
- Syslog and SNMP are both used for monitoring and troubleshooting of devcies. They are complementary, but their functionalities are different. 

### Syslog Message Format: 
seq:time stamp: %facility-severity-MNEMONIC: description 

- Facility: The type of process that generated the message (e.g., kernel, mail system, etc.)
- Severity: The level of importance of the message (e.g., emergency, alert, critical, error, warning, notice, informational, debug)
- MNEMONIC: A short code that identifies the specific event that occurred (e.g., LINK-3-UPDOWN for an interface going up or down)
- Description: A human-readable description of the event that occurred.


### Syslog Severity Levels:
| Level | Keyword       | Description                                     |
| ----- | ------------- | ----------------------------------------------- |
| 0     | Emergency     | System is unusable                              |
| 1     | Alert         | Action must be taken immediately                |
| 2     | Critical      | Critical conditions                             |
| 3     | Error         | Error conditions                                |
| 4     | Warning       | Warning conditions                              |
| 5     | Notice        | Normal but significant condition (Notification) | // note: In RFC it is notice, in cisco it is notification. 
| 6     | Informational | Informational messages                          |
| 7     | Debugging     | Debug-level messages                            |

MNEMONIC: *Every Awesome Cisco Engineer Will Need Ice Cream Daily.* 
MNEMONIC: *Emergencies are Critical Even when nobody is dying.* 


### Syslog logging locations: 
- Console line: Syslog messages will be displayed in the CLI when connected to the device via the console prot. by default, all mesasage(level 0 - level 7) are displayed. 
- VTY lines: Syslog messages will be displayed in the CLI when connected to the device via Telenet/SSH. Disabled by default. 
- Buffer: Syslog messages will be stored in the device's RAM. By default, all messages (level 0 - level 7) are stored in the buffer. COMMAND: show logging
- External Syslog Server: You can configure the device to send Syslog messages to an external server. 

### Syslog Configuration:
- `logging console [level]` // logging to the console line
- `logging monitor [level]` // this is for VTY lines, and it is disabled by default.
- `logging buffered [size in byted of buffer] [level]` // logging to the buffer (buffer is circular)
- `logging host [IP address of syslog server] [level]` // logging to an external syslog server 
- `logging trap [level]` // this is for SNMP traps, and it is disabled by default. (for external server)
- `service timestamps log datetime msec` // this is for adding timestamps to the log messages, and it is disabled by default.
