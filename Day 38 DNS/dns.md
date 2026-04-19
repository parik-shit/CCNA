#### Purpose: 
- DNS is used to resolve human-readable names (googl.com) to IP addresses. 
- Machines suhc as PCs don't use names, they use addresses.
- Names are much easier for us to remember than IP addresses.
- When you type 'youtube.com' into a web browser, your device will ask a DNS server for the IP address of youtube.com.
- The DNS servers your device uses can be manually configured or learned via DHCP.

#### Useful commands: 
- `ipconfig /all` to see the DNS servers configured on a Windows device.
- `ipconfig /displaydns` to see the DNS cache on a Windows device.
- `ipconfig /flushdns` to clear the DNS cache on a Windows device. 

### DNS in Cisco devices:
- `ip dns server` configure R1 to act as a DNS server. 
- `ip name-server [ip address]` configure R1 to use the specified DNS server(R1 will use this if a record is not found in the local DNS database).
- `ip host [hostname] [ip address]` to add a record to the local
- `ip domain-lookup` to enable DNS lookup on R1.
- `show hosts` to see the local DNS database on R1.
- `ip domain-name [domain name]` to configure the default domain name for R1. This is used when R1 is trying to resolve a hostname without a domain name. For example, if R1 is trying to resolve 'www' and the default domain name is 'example.com', R1 will try to resolve 'www.example.com'.

MENTAL MODEL: In the lecture you are confused between when a "router" acts as a DNS server and when it acts as a DNS client. A router can be both at the same time. When you configure `ip dns server` on a router, it will act as a DNS server and will use its local DNS database to resolve hostnames. When you configure `ip name-server [ip address]` on a router, it will act as a DNS client and will use the specified DNS server to resolve hostnames that are not found in the local DNS database.
