# Cisco Catalyst 3850 switch

## Imaging the switch

Download cat3k_caa-universalk9.16.12.05b.SPA.bin from:

[https://software.cisco.com/download/home/284455434/type/282046477/release/Gibraltar-16.12.5b](https://software.cisco.com/download/home/284455434/type/282046477/release/Gibraltar-16.12.5b)

Note: You will have to have an account with cisco which may take upto a day for them to set up and verify

1. Take an usb formatted in FAT and move .bin file to it

    This means one of 2 things:

    a. either take a 4gb usb and format it to FAT 

    b. you must make a 4gb partition on an usb that is larger than 4gb. To create a 4gb partition, you should open a cmd in administrator mode, then do the following:

   1. diskpart

   2. list disk

   3. select disk <INDEX OF USB DISK>
    
   4. clean

   5. create part primary size=4000

   6. active

   7. Then format the drive in file explorer to FAT. 
   8. Put the file on to the usb drive. It should be the only file on the usb.

   9. put it in the switch
   
2. Now on the switch do the following:
run dir to see if usbflash is visible

dir usbflash0:/
you should see a cat3K_ file and you will have to type in the following:

boot usbflash0:cat3k_caa-universalk9.16.12.05b.SPA.bin

set password as the csg default password

once in the terminal ">" run: 

1. **enable**
2. copy usbflash0:cat3k_caa-universalk9.16.12.05b.SPA.bin bin
3. configure terminal
4. boot system switch all flash:cat3k_caa-universalk9.16.12.05b.SPA.bin
5. ctrl + z
6. show run | inc boot
7. exit

You will now have the device "reimaged"

- Set vlan ip

Note: this may be done in set up

- VTP

DO NOT USE!

Using VTP has some sharp edges if not used carefully, like wiping all vlan configs.

Vlans should be configured manuelly on each switch.

See the configuring ports section below.

- Set up web interface
1. enable 
2. configure terminal
3. username admin privilege 15 password cisco
4. ctrl + z
5. exit
6.  save running config (directions below)
- Create vlan

- add interfaces to a vlan
- Erase config

enable

erase wr

reload

Follow initialization setup except make sure to run:

delete vlan.dat 

- Saving running config

enable

Copy running-config startup-config

- Set ethernet port up as a trunking port

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7491ee65-73b4-4f26-94e2-edf5191281f5/cisEthCap.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7491ee65-73b4-4f26-94e2-edf5191281f5/cisEthCap.png)

if logging in does not work.
ip http authentication aaa login-authentication AUTH_LIST

Notes:

port trunking on d-link is actually port agrregration

Do not turn on port fast:

PA:
Create dhcp on Labs for [http://192.169.0.1/16](http://192.169.0.1/16)

Vlans

1 - default - none

2 - INTERNAL - 192.168.1.0/24 

3 - UPS - 192.168.7.1/24

4 - DMZ - 192.168.3.1/24

5 - BACKUP - 192.168.9.1/24

6 - ILO - 192.168.50.0/24 

7 - PRODAPPCLUSTER - 192.168.40.0/28

8 - CSLABS-LABS - 172.17.0.0/20

9 - DEVAPPCLUSTER - 192.168.41.0/28

Create policy for Dev webserver

192.168.1.252 - rack3 

192.168.1.251 - rack1 procurve

192.168.1.248 - rack2 cisco switch

192.168.1.249 - backhaul switch

TODO AT CSG: 

Fix cisco switch

Set up cluster for internal

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/475949ad-bef2-46e0-8dff-3c9271f46650/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/475949ad-bef2-46e0-8dff-3c9271f46650/Untitled.png)

config svi
vlan database

vlan 2

configure terminal

interface Vlan2

ip address 10.1.2.1 255.255.255.0

no shutdown

1/0/13 192.168.156.99

Note: their is a management port in the back