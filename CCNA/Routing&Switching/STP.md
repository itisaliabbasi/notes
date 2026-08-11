# STP

> *STP => Spanning Tree Protocol*
> *BPDU => Bridge Protocol Data Unit* -> packets sent by STP

## STP Versions

1. 802.1D: traditional STP
2. 802.1W: Rapid STP
3. 802.1S: Multiple STP -> Open Standard
4. PVST -> Cisco Propriatary
5. RPVST -> Cisco Propriatary

> *RSTP => Rapid STP*
> *MSTP => Multiple STP*
> *PVST => Per VLAN STP*
> *RPVST => Rapid Per VLAN STP*

> `show spanning-tree` shows info of STP, to see root of STP run `show spanning-tree root`

> Root Bridge Election: The switch with lowest Bridge ID (Priority + VLAN number + Base MAC Address) becomes Root Bridge

### Port Roles

1. Root Port: forwarding towards the root (on non-root switches), chosen based on lowest cost
2. Designated Port: forwarding away from root
3. Alternate\Backup: the port thats shutdown so loop doesnt occure and it will be on when current port fails

### Port States

1. FWD: forwarding
2. BLK: blocking/disguarding

## STP Config

```
conf t
spanning-tree portfast default // To get rid of the delay of STP on access ports
spanning-tree mode rapid-pvst
spanning-tree vlan [x] root [primary|secondary|x] // To set a switch as root or secondary root in a VLAN or give it an static manual priority
show spanning-tree root
```

## STP Hardening

> on access ports BPDU guard must be enabled so no switch can be connected to them. (puts port in err-diable state) or we can use BPDU filter to ignore BPDUs and there would be no err-disable problem
> on trunk ports of root bridge switch, Root guard must be enabled so no switch other than that can become root bridge.

```
conf t
spanning-tree portfast edge bplufilter default //enable bpdufilter by default on portfast interfaces
int ra gig 0/0-3 //on access ports
	spanning-tree bpuduard enable //err-disable if switch is connected to it
	spanning-tree bpdufilter enable //ignore BPDU entirly
	exit
int gig 0/4 //on trunk ports
	spanning-tree guard root
	exit
```

> Loop Guard on ports is for when there is a blocked port and BPDUs stop comming to that port, it wont change to forwarding, instead it will go to loop inconsistancy mode. because the link may have problem.

![[Loop Guard.excalidraw|800]]

```
conf t
int gig 0/5
	spanning-tree guard loop //enable loopguard on an interface
	exit
spanning-tree loopguard default //enable it globally
```
