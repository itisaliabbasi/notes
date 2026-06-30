# VLAN's & L2

## L2 Switching

> CAM or MAC Address Table: has MAC address connecter to each port and its VLAN and Type, its filled up by MAC learning process and each record has aging of 300 seconds.

```
show mac address-table
show mac address-table aging-time
mac address-table aging-time [x]
```

> *ARP => Address Resolution Protocol* -> has a table that has MAC's and their relative IP address. in order to fill its table, ARP flooding happens by ip that we dont know its MAC, its relative host responds with its MAC.

> [!NOTE]
> Flooding VS Broadcast: flooding happens on all ports except the one it came from, but broadcast happens on all ports of same VLAN.

## VLAN

### Switch

```
conf t
vlan 100
name User-vlan
exit
int vlan 100
ip address 192.168.1.1 255.255.255.0
description usr-vlan
exit
int gig 1/0/1
switchport mode access
switchport access vlan 100
```

### Router

![[InterVLAN Routing.excalidraw|800]]

```
SW-1:
conf t
int gig 1/0/24
switchport trunk encapsulation dot1q
sw mode trunk
sw trunk native vlan <x>
sw trunk allowed vlan none/remove/except/all/add/<x,y,z>
exit
int gig 1/0/1
sw mo acc
sw acc vlan 1
exit
int gig 1/0/2
sw mo acc
sw acc vlan 2
exit
do sh vlan brief

R-1:
conf t
int gig 0/1
int gig 0/1.1
encapsulation dot1q 1
ip address 192.168.1.1 255.255.255.0
exit
int gig 0/1.2
encapsulation dot1q 2
ip address 192.168.2.1 255.255.255.0
exit
do sh ip int brief
do sh vlans
```

## Switch Inital Config

```
write erase //same as 2 line below
delete nvram:startup-config
erase startupconfig
delete flash:vlan.dat
reload 
enable secret P@ssw0rD
conf t
line console 0
logging synchronous //re prints our command in new line when a log is shown in terminal
exit
no ip domain-lookup
no service timestamps debug //only in lab not production
no service timestamps log //only in lab not production
exit
int vlan MGMT
ip address 192.168.1.1 255.255.255.0
exit
ip default gateway 192.168.1.254
ip routing //if we want intervlan routing on MLS
write //same as below
copy runnig-config startup-config
```

> [!NOTE]
> `who` shows all the logged in users.
> `show users` shows all the users.

## Voice VLAN

```
conf t
int gig 1/0/1
sw mo acc
sw acc vlan [x]
sw voice vlan [y]
exit
```
