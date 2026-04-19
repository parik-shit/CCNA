### Console Port Security: Login

- By default, no password is needed to access the CLI of a Cisco IOS device via the console port.
- You can configure a password on the conole line.
- A user will have to enter a passwrod access the CLI via the console port.
- Alternatively, you can cconfigure the conole line to required users to login using one of the configured usernames on the device. 

### Console Port Security: Login Local
- `username [username] password [password]` // this is for creating a local user on the device. You can create as many users as you want.
- `line console 0` // this is for entering the console line configuration mode.
- `login local` // this is for configuring the console line to require users to login using one of the configured usernames on the device.
- `end` // this is for exiting the configuration mode.

### Layer 2 Switch - Management IP 
- Layer 2 switches don't perform packet routing and don't build a routing table. They aren't routing aware. 
- However, you can assign an IP address to an SVI to allow remote connections to the CLI of the switch (using Telenet or SSH).

Configure an SVI:
- `interface vlan [vlan number]` // this is for entering the SVI configuration mode. 
- `ip address [IP address] [subnet mask]` // this is for assigning an IP address to the SVI.
- `no shutdown` // this is for enabling the SVI. By default, the S
- `ip default-gateway [IP address of default gateway]` // this is for configuring the default gateway for the switch. This is needed for remote access to the switch if the NMS is on a different subnet than the switch.

### Telnet
- Teletype Network is a protocol used to remotely access teh CLI of a remote host. 
- Telnet was developed in 1969. 
- Telenet has been largely replaced by SSH, which is more secure. 
- Telenet sends data in plain text. No encryption is used, so it is vulnerable to eavesdropping. 

### SSH 
- Uses port 22. 

#### SSH Configuration: RSA Keys 
- To enable and use SSH, you must generate an RSA public and private key pair. 
- The keys are used for data encryption/decryption, authentication, etc. 
- `ip domain-name [domain name]` // this is for configuring the domain name of the device. This is needed for generating the RSA keys.
- `crypto key generate rsa` // this is for generating the RSA keys. 
- `show ip ssh` // this is for verifying that SSH is enabled and to see the details of the SSH configuration.

#### SSH Configuration: VTY Lines
- `enable secret [password]` // this is for configuring the enable secret password, which is used to protect access to privileged EXEC mode. This is needed for SSH access.
- `ip ssh version 2`
- `line vty 0 15` // this is for entering the VTY line configuration mode. This is needed for SSH access.
- `login local` // this is for configuring the VTY lines to require users to login using one of the configured usernames on the device. This is needed for SSH access.
- `transport input ssh` // this is for configuring the VTY lines to only allow SSH

#### SSH Configuration: Access Control
- You can use an ACL to restrict which hosts can access the device via SSH.
- `access-list [number] permit [source IP address] [wildcard mask]`

#### Connecting to a Device via SSH:
- `ssh -l [username] [IP address of device]` or `ssh [username]@[IP address of device]`
