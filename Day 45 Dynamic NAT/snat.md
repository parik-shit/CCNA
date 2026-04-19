### Dynamic NAT: 
- It helps multiple private IP to share a single public IP address. 
- We do this by configuring a pool of public IP addresses and then mapping them to the private IP addresses as needed.
- The mapping is based on ACL. 

#### Configuration: 
`ip nat pool <pool-name> <start-ip> <end-ip> netmask <netmask>`
- This command creates a pool of public IP addresses that can be used for NAT translation. 
- The `<pool-name>` is a name you choose for the pool, `<start-ip>` and `<end-ip>` define the range of public IP addresses in the pool, and `<netmask>` specifies the subnet mask for the pool.

`access-list <acl-number> permit <source-ip> <wildcard-mask>
- This command creates an access control list (ACL) that defines which private IP addresses are allowed to be translated. 
- The `<acl-number>` is a unique identifier for the ACL, `<source-ip>` is the private IP address or range of addresses you want to allow, and `<wildcard-mask>` is used to specify the range of IP addresses in the ACL.	

`ip nat inside source list <acl-number> pool <pool-name>
- This command tells the router to use the specified ACL to determine which private IP addresses can be translated and to use the specified pool of public IP addresses for the translation. 
- The `<acl-number>` is the same number used in the previous command to identify the ACL, and `<pool-name>` is the name of the pool of public IP addresses created earlier.	

### PAT (Port Address Translation): MANY-TO-ONE NAT
- It allows multiple private IP addresses to share a single public IP address by using different port numbers at the same time.
- The configuration is similar to Dynamic NAT, but instead of using a pool of public IP addresses, we use a single public IP address and different port numbers to differentiate between the private IP addresses.
#### Configuration:
`ip nat inside source list <acl-number> interface <interface-name> overload`
- This command tells the router to use the specified ACL to determine which private IP addresses can be translated and to use the specified interface's IP address for the translation. 
- The `<acl-number>` is the same number used in the previous command to identify the ACL, and `<interface-name>` is the name of the interface that has the public IP address assigned to it. The `overload` keyword allows multiple private IP addresses to share the same public IP address by using different port numbers.

#### Configuration PAT on interface:
`interface <interface-name>`
`ip nat inside`
- This command is used to configure the interface that is connected to the private network as an inside NAT interface. 
- The `<interface-name>` is the name of the interface that is connected to the private network. This tells the router that any traffic coming from this interface should be considered for NAT translation.		
`interface <interface-name>`
`ip nat outside`
- This command is used to configure the interface that is connected to the public network as an outside NAT interface. 
- The `<interface-name>` is the name of the interface that is connected to the public network. This tells the router that any traffic going out of this interface should be considered for NAT translation.	
`ip nat inside source list <acl-number> interface <interface-name> overload`
- This command tells the router to use the specified ACL to determine which private IP addresses can be
translated and to use the specified interface's IP address for the translation.
