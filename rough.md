network 172.20.20.0 0.0.0.255
network 10.0.0.0 0.0.255.255 area 0  

ip access-list standard "" 
permit 192.168.1.1 0.0.0.0:wq

deny 192.168.1.0 0.0.0.255
deny 192.168.2.1 0.0.0.0
