# How To TFTP Recover A B093, B097, or B098 Tripp Lite Console Server

## Purpose
To demonstrate how an otherwise unresponsive, misconfigured, or misbehaving console server could be recovered using TFTP.
It is recommended to first attempt a hard reset of the console server before following this process. You can find hard reset steps here (Option 2):
https://www.tripplite.com/support/how-to-reset-a-console-servers-factory-default-settings

## Requirements
*	TFTP server 
*	B093, B097, or B098 console server
*	Firmware file
*	Access to console server’s console port (port #1 for B093s)
*	Network connectivity between console server and TFTP server

## Steps

1. **Load the firmware image to the root directory of the TFTP server**

2.  **Connect via serial to the console server's console port**
	
	The B097 and B098 have dedicated console ports. For the B093, use serial port 1.
	_Link to serial port FAQ_

3.	**Power the console server on while holding the erase button to place the console server into _network recovery mode_**

4. **Input a static IP address, subnet, and gateway (assumes no DHCP address obtained, skip to step 4 if DHCP address obtained)**
	
	Below are the commands required to give the console server an IP address of 192.168.1.248, subnet of 255.255.255.0, and gateway of 192.168.1.1
	```
	Marvell>> set ipaddr 192.168.1.248
	Marvell>> set netmask 255.255.255.0
	Marvell>> set gatewayip 192.168.1.1
	```

5. **Input the name of the firmware image**
	
	Below is the command required to use a firmware image named _b093-4.1.1u1.flash_ that is in the TFTP server's root directory.
	```
	Marvell>> set bootfile b093-4.1.1u1.flash
	```
	
6. **Input the TFTP server's IP address**
	
	Below is the command required to point to a TFTP server at IP address 192.168.1.5
	```
	Marvell>> set serverip 192.168.1.5
	```
	
7. **Boot from TFTP using the _tftpboot_ command**
	```
	Marvell>> tftpboot
	```
	
8. **Use _gofsk_ to load the firmware image downloaded via TFTP**
	
	After entering the _tftpboot_ command, the console server will report a load address such as _0x2000000_ in the example below:
	```
	Marvell>> tftpboot
	Using egiga0 device
	TFTP from server 192.168.1.5; our IP address is 192.168.1.248
	Filename 'b093-4.1.1u1.flash'.
	Load address: 0x2000000
	```
	Copy the load address value and input it after the gofsk command as in the example below:
	```
	Marvell>> gofsk 0x2000000
	```
	
9. **Wait for the console server to boot, then login and reset to factory default settings**
	
	Within a few minutes, the console server will reboot and offer a login prompt. Login using the root username and password (default is root/default) and reset the console server using the _flatfsd -i_ command.
	```
	B093-008-2E4U-V login: root
	Password:
	# flatfsd -i
	```

10. **The console server will reset and reboot. It will be available within a few minutes. Once the console server reboots, reconfigure it to the desired settings and use normally.**

## Questions?

If you have further questions, contact Tripp Lite Technical Support at 773-869-1234 (option 3) between 7 a.m. to 6 p.m. Central Time Mon-Fri or via email at techsupport@tripplite.com
	
	
	
	

