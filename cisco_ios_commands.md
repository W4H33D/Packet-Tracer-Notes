# Cisco IOS Commands


### Cisco IOS Modes

1. User EXEX MODE(Low Privileged)
2. Privileged EXEC MODE(High Privileged)

### Navigate b/w IOS MODE

1. **enable:**
	In user exec mode when you type the **enable** command and enter it then you will move into privileged exec mode.
	```
	Router> enable
	Router# 
	```
2. **disable**:
	In privileged exec mode when you type the **disable** and press enter then you will move into user exec mode.
	```
	Router# disable
	Router>
	```
3. **configure terminal**:
	In privileged exec mode in order to configure the router/switch you have to move into global configuration mode. So to do this you type **configure terminal** and press enter then you will move into global configuration mode.

4. **line console 0**:
	In global configuration mode if you type **line console 0** and press enter then you will move in to *sub configuration* mode of `management interface of console port`
5. **line vty 0 15**:
	In global configuration mode if you type the above command then you will move into sub configuration mode of `virtual terminal management`. That are use in remote management
```
Note: You can directory move one sub configuration mode to another
```
6. **interface vlan 1**:
	In global configuration mode or sub configuration mode you can type the above command to go into `Interface Configuration Mode of Virtual LAN`
7. **interface fastethernet 0/1**:
	In global configuration mode or sub configuration mode you can type the above command to go into `Interface Configuration Mode Physical LAN`
8. **end,exit**:
	- **end**
		When you are in sub-configuration mode or interface mode and you want to move all the way back into priv-exec mode so we can use the above command or also we can use `CTRL+z` keyboard shortcut.
	- **exit**
		When you are in sub-configuration mode or interface mode or global configuration mode and want to back one step so we can use the above command.

### Basic IOS Command Structure

```
[Prompt][Command][Space][Keywords or Arguments]
i.e.
Switch>show ip protocols
or
Switch>ping 192.168.10.5
```
- **Keyword**: This is a specific parameter defined in the operating system
- **Arguments**: This is not predefined; it is a value or variable defined by the user.

### Cisco IOS show commands.
To view different configuration we use **show** commands. Some of them are flows.
- **show running-config**:
	This command is use to view current configuration on the router/switch.This will show you all the commands that are configured in the router/switch. That includes its version information,interface information,hostname,and password etc.

- **show interfaces**:
	This command show all the interfaces on the router/switch.
- **show arp**:
	This will show you the arp table for the router/switch.
- **show ip route**:
	This will show you the routing table of the router.
- **show protocols**:
	This will show you both global and specific information for any layered three interfaces.
- **show version**:
	This will show you information about the operating system and hardware information.

### Basic Switch Configuration

1. **Configure Device Name**:
	In Global exec mode use **hostname** *name* command to change the device(router/switch) name.
	e.g:
	```
	switch(config)#hostname Switch1
	switch1(config)#
	```
2. **Secure privileged EXEC mode**:
	In global configuration mode use **enable secret** *password*
	command to set/changed privilege exec mode password for router/switch. After that user must write the password to enter in privileged exec mode.
	i.e:
	```
	switch1(config)# enable secret Super_Secure_Password_123
	```
3. **Secure user EXEC mode**:
	To set/change password of user exec mode we use the following command in global configuration mode of the router/switch.
	1. **line console 0**
	2. **password** *password*
	3. **login**
	i.e:
	```
	switch1(config)# line console 0
	switch1(config-line)# password Super_Secure_User_Password123
	switch(config-line)# login
	```
4. **Secure remote SSH/Telnet access**:
	Use the following commands in global configuration mode or sub configuration mode.
	1. **line vty 0 15**
	2. **password** *password*
	3. **transport input** *{ssh |telnet | none | all}*
	4. **login**
	i.e:
	```
	switch1(config)# line vty 0 15
	switch1(config-line)# password Super_Secure_Telnet_Password123
	switch1(config-line)# transport input ssh
	switch1(config-line)# login
	```
5. **Encrypt All Password**:
	Use **service password-encryption** in global configuration mode to encrypt all the running configuration password of the router/switch.
6. **Configuring Switch Virtual Interface**:
	Use the following command in global configuration mode or sub-configuration mode.
	1. **interface vlan 1**
	2. **ip address** *ip address* *subnet-mask*
	3. no shutdown
	i.e:
	```
	switch1(config)# interface vlan 1
	switch1(config-if)# ip address 192.168.10.11 255.255.255.0
	switch1(config-if)# no shutdown
	```
7. **Set A banner**:
	Use the following command in global configuration mode to set the banner in router/switch.
	- **banner motd** *delimiter* *message* *delimiter*
	`Note: delimeter is any special character`
	i.e:
	```
	switch1(config)# banner motd #No UnAuthorized Access is Allowed# 
	```
8. **Save the Configuration**
	To save the configuration we use the following commands in Privileged EXEC Mode of the router/switch.
	- **Copy running-config startup-config**
	The above command will save the running configuration in NVRAM and automatically configure if Switch/Router restarts.
	- **Copy startup-config flash**
	The above command will save the startup configuration in flash storage and will be use if startup configuration will corrupt.
	i.e:
	```
	switch1# copy running-config startup-config
	Destination filename [startup-config]?
	Building configuration
	[OK]
	switch1#
	```
### Basic Router Configuration
The Router configuration most of the commands are same as the switch configuration.
```
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(Config)# enable secret Super_Secure_Privilege_Password123
R1(Config)# line console 0
R1(Config-line)# password Super_Secure_USER_Password123
R1(Config-line)# login
R1(Config-line)# line vty 0 4
R1(Config-line)# password Super_Secure_Telnet_Password123
R1(Config-line)# transport input {telnet | ssh | none | all}
R1(Config-line)# login
R1(Config-line)# exit
R1(Config)# service password-encryption
R1(Config)# banner motd "UnAuthorized Access is Not Allowed!" 
R1(Config)# copy running-config startup-config
R1(Config)# copy startup-config flash
```
### Secure Remote Access
From above we setup a telnet connection that are not secure but here we setup a secure remote connection using SSH. Some of the command are same as shown above.
```
R1(Config-line)# line vty 0 4
R1(Config-line)# password Super_Secure_Telnet_Password123
R1(Config-line)# transport input ssh
R1(Config-line)# login
```
After that we have to use some additional command for SSH and that are following.
1. Verify SSH support
First verify the switch/router have SSH access with the command **show ip ssh** if that command is unrecognized then the switch/router does't support SSH
2. Configure The IP domain
We have to set a domain for the router/switch. using the command **ip domain-name** *name* in global configuration mode.
i.e:
switch# ip domain-name hrswitch.io
3. Generate RSA key pairs
SSH use Public/Private Key to encrypt and decrypt all the communication. So we have to generate a public and private key. We can do this using the following command.
**Crypto key generate rsa**
After executing the above command they ask for key length and normally we use 1024 bit keys. Longer keys are more secure but they required more time to generate and process.
4. Configure User Authentication
To connect to the SSH server we must have to assign a user to which they connect so we have to generate a local user and password pair. We can generate it using the following command.
**username** *username* **secret** *password*
We have to enter the above command in global config mode.
5. Configure the vty lines
As we setup SSH connection that are more secure then SSH so we have only permit SSH connections and prevents all non-SSH connections.
We previously enable SSH using the following command.
**transport input ssh**
So we have to enable ssh logins to do this we use the following command.
**line vty 0 15**
**login local**
6. Enable SSH version 2
SSH version 1 have common vulnerabilities so we must use the latest version of SSH if we have. So we can enable SSH version 2 with the following command.
**ip ssh version 2**
This command is run in global configuration mode
