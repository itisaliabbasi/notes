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

```
