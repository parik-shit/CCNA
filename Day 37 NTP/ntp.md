#### Importance of time

- All devices have an internal clock
- COMMAND: `show clock`
- COMMAND: `show clock detail` to check source of time
- The internale hardware clock of a device will drift over time, so it is not the ideal time source.
- The most important reason for time to be accurate is to have accurate logs for troubleshooting.

#### Manual Time configuration:

COMMAND: `clock set`

COMMAND: `calendar set` to configure hardware clock
COMMAND: `show calendar` to see hardware clock

COMMAND: `clock read-calendar` software clock will sync with respect to hardware
COMMAND: `clock update-calendar` hardware clock will sync with respect to software clock

COMMAND: `clock timezone [timezone]` to configure timezone

#### Daylight Saving Time Configuration: (this is not complete yet)

COMMAND: `clock summer-time`

_NOTE: hardware and software clock can be configured seprately (also hardware is the default source)_

---

### Network Time Protocol
- Manually configuring the time on devices is not scalable. 
- The manually configured clocks will drift, resulting inaccurate time. 
- NTP(Network Time Protocol) allowes automatic syncing of time over a network. 
- NTP clients request the time from NTP servers. 
- A device can be an NTP server and an NTP client at the same time. 
- NTP allows accuracy of time with ~1 millisecond if the NTP server is in the same LAN, or within ~50 milliseconds if connecting to the NTP server over a WAN/ the internet. 
- Some NTP server are "better" than others. The 'distance' of an NTP server from the original *reference clock* is called *stratum*. 
- NTP uses UDP *port 123* to communicate. 

### Reference Clocks 
- A reference clock is usually a very accurate time device like an atomic clock or a GPS clock. 
- Reference clocks are *stratum 0***
- Stratum 1 NTP servers are directly connected to reference clocks.
- Stratum 2 NTP servers are connected to stratum 1 NTP servers,
- Stratum 3 NTP servers are connected to stratum 2 NTP servers, and so on.**
- Stratum 15 is the lowest stratum, and stratum 16 means the device is unsynchronized.
- Devices can also 'peer' with devices at the same stratum to provide more accurate time. 

### NTP Configuration:
- To configure a device as an NTP client, use the `ntp server [ip address]` command.
- To see associated NTP servers, use the `show ntp associations` command.
- To see the status of NTP synchronization, use the `show ntp status` command
- to sync hardware clock with NTP server, use the `ntp update-calendar` command.

