# LAN Switching Fundemantals

## MAC Table

> `show mac address-table` to see MAC table
> `show mac address-table address <mac-address>` to see if a MAC is in our MAC table or not
> `show mac address-table count` to see # of MACs learned by switch
> `mac address-table aging-time <secs>` we can change aging time of MACs (not recommended to change)
> `mac address-table static <mac-address> vlan <vlan-id> interface <type-number>` to add an static MAC to MAC table -> always static entries win, when switch is forwading frames

## TCAM and SDM

> *TCAM => Ternary Content Addressable Memory* -> erased at poweroff, each unit in this memory can store 3 vlaues: 0=true , 1=false , x=dontcare , used to store routes, ACL entries, IPs, …
> each entry in TCAM is stored as VMR triplets.
> *VMR => Value, Mask, Result*
> *SDM => Switching Database Manager*

![[Screenshot From 2026-08-28 14-36-17.png]]

![[Screenshot From 2026-08-28 14-38-47.png]]

 > `show sdm prefer` to see current SDM template
 > `show sdm prefer ?` to see other SDM templates
 > `show sdm prefer <template-name>` to see details on another SDM template and then `reload` to change into chosen template

## VLANs

> *VLAN => Virtual Local Area Network*
> `vlan <id>` creates a VLAN globally (not created until we write `exit`)
> `name <string>` in that VLAN gives it a name
> `show vlan`, `show vlan brief` show the list of VLANs and their assigned ports

![[Screenshot From 2026-08-28 15-05-37.png]]

### Assign VLAN to Interface

```
conf t
	interface <type/value>
		switchport mode access
		switchport access vlan <id>
```

### Range of VLANs

```
vlan <x>-<y> //creates a continues range of VLANs
vlan <x>,<y>,<z> //creates multiple specified VLANs
no vlan <x>-<y> //delete a range of VLANs
no vlan <x>,<y>,<z> //delete multiple VLANs
```

> [!NOTE]
> `show arp` shows arp table contents

## Trunks

> *ISL => Inter Switch Link* -> legacy cisco trunking protocol
> 802.1q is the modern open standard trunking protocol, native VLAN is VLAN1 and its recommended to change it to another unused VLAN
> `show interfaces trunk`, `show interface status`, `show interface <type/value> switchport` to see trunk interfaces status and their allowed VLANs

![[Screenshot From 2026-08-28 15-13-04.png]]

### Trunk Config

```
conf t
	interface <type/vlaue>
		switchport trunk encapsulation <isl/dot1q> //if switch supports both ISL & 801.1q
		switchport mode trunk
		switchport trunk native vlan <id> //this VLAN must already exist so first, it must be created
		switchport trunk allowed vlan <add|all|except|none|remove|id> <range-of-vlans>
		exit
	vlan dot1q tag native //to do tagging even on nativ vlan frames
```

![[Screenshot From 2026-08-28 15-24-21.png]]

### DTP

> *DTP => Dynamic Trunking Protocol*

![[Screenshot From 2026-08-28 15-27-59.png]]

```
conf t
	interface <type/value>
		switchport mode <trunk|desirable|auto>
		switchport nonegotiate //to disable DTP
```

## VTP

> *VTP => VLAN Trunking Protocol* -> only flows through trunk ports

![[Screenshot From 2026-08-28 15-36-14.png]]

### VTP Modes

1. versions 1&2:
	1. server: create/delete VLANs and give them to clients
	2. client: cant create/delete VLANs and only gets them from server
	3. transparent: create/delete VLANs locally and pass the VTP traffic trough itself
	4. off: shutdown VTP
2. version 3:
	1. server(secondary): cant create/delete VLANs, but can become primary server
	2. primary server: create/delete VLANs and give them to clients
	3. client: cant create/delete VLANs and only gets them from server
	4. transparent: create/delete VLANs locally and pass the VTP traffic trough itself
	5. off: shutdown VTP

> [!NOTE]
> In VTP version 1&2, when we want to configure PrivateVLANs, we should put it to transparent mode.

### VTP Parameters

1. domain name
2. authentication
3. configuration revision: becomes 0 by changing to transparent mode or changing domain name

![[Screenshot From 2026-08-28 16-05-14.png]]
![[Screenshot From 2026-08-28 16-06-18.png]]
![[Screenshot From 2026-08-28 16-06-50.png]]

### VTP Config

1. Version 1&2:

```
conf t
	vtp domain <name>
	vtp version <1|2>
	vtp mode <client|server|transparent>
	vtp password <pass>
```

2. Version 3:

```
conf t
	vtp domain <name>
	vtp version 3
	vtp mode <client|server|transparent>
	vtp primary <force|mst|vlan> \\to make switch primary server
	vtp password <pass> hidden \\hashes password
	vtp password <hashed-pass> secret \\to use hashed pass for another switch
```

```
VTP Verification:
	show vtp status
	show vtp counters
	show vtp password
	show vtp interface
```

## Etherchannel

 > 802.3ad is the protocol IEEE standard
 > allows up to 8 active links in a channel
 > `show etherchannel load-balance` shows the load balancing algorithm
 > `port-channel load-balance <algorithm>` changes loadbalance algorithm of ehterchannel

## Protocols

1. on: static
2. PAgP
![[Screenshot From 2026-08-28 18-39-56.png]]

3. LACP
![[Screenshot From 2026-08-28 18-41-25.png]]

> *PAgP => Port Aggregation Protocol*
> *LACP => Link Aggregation Control Protocol*

### Etherchannel Config

```
conf t
	interface range <type/value>
		channel-group <number> mode <active/passive|on|auto/desirable>
		exit
	intreface port-channel <number>
		port-channel min-links <vlaue> //min links in order to make this interface active
		lacp max-bundle <1-8> //max number of links that can be in LACP
show etherchannel summary //to see its info
```
