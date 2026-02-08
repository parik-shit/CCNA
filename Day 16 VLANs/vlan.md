# Native VLAN on a router (ROAS)

Best Practice: Set the native vlan to an unused VLAN 
Command: `native vlan [VLAN number]`

The traffic that goes through native VLAN is untagged. 
Purpose: To let the forward the traffic that is untagged (basically not coming from a VLAN). 


## There are 2 methods to configure a VLAN 

1. Command: `native vlan [VLAN number] native`
2. Configure the IP address for the native VLAN on the router's physical interface (the encapsulation vlan -id command is not necessary)


# Layer 3 (Multilayer) Switches
- It is layer 3 aware 
- Interfaces can be config
- Intervlan routing can be done
- cofigure routes like a router 
- basicall, it is capable of both routing an switching 


## Inter-VLAN Routing via SVI 

- `ip routing`
- `int [interface name]`
- `no switchport`
- `ip address [ip] [subnet mask]`
- set a default route pointing to the router -> `ip route 0.0.0.0/0 [ip of router interface connected to switch]`

Bonus: use command `show int status`

- config int on switch 
    int [interface name]
    int vlan [vlan number]
    ip address [ip of default gateway of that VLAN] [subnetmask]
    no shutdown




