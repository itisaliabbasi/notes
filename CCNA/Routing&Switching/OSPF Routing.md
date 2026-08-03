# OSPF Routing

> Area 0 in OSPF is also called backbone area
> Each router in OSPF must have unique RID
> *RID => Router ID*

## OSPF Info between Neighbors

> *STTAMP => Subnet, Timers, networkType, Area, MTU, Password*

## DR

> *DR => Designated Router*
> *BDR => Backup Designated Router*

### Per Ether Subnets

| `#` of subnets |      Routers      |
| :------------: | :---------------: |
|       1        |        DR         |
|       2        |      DR, BDR      |
|       3        | DR, BDR, DR-Other |
|      …       |                   |

### DR Election

1. highest int priority
2. highest router ID

> [!NOTE]
> Serial links are point to point and doesnt have DR, BDR

## OSPF Initial Config

```
router ospf 1
router-id x.x.x.x
network 0.0.0.0 255.255.255.255 area 0 \\has all the interfaces in OSPF
show ip protocols
show ip ospf interface brief
show ip ospf neighbor
show ip ospf interface gig 0/0
```

### Change OSPF Timers & Network Type

```
int gig 0/0
ip ospf hello-interval <x>
```

### OSPF RID Selection

1. RID configuration
2. highest ip on loopback interfaces
3. highest ip on an interface

## OSPF Config

```
conf t
int gig 0/1
ip ospf 1 area 0 //explicit ospf
ip ospf cost <x> //change int cost manually
exit
sh ip ospf int brief
sh ip route
sh ip route ospf
router ospf 1
router-id 1.1.1.1
auto-cost refrence-bandwidth 10000 //in Mbits (10000->10Gb)
network 192.168.1.0 0.0.0.255 area 0 //implicit ospf network in ospf
network 192.168.20.1 0.0.0.0 area 0 //advertize a single host in ospf
default-information originate //advertise default route in ospf
exit
sh ip ospf neighbor summary
 
```

> [!NOTE]
> `default-information originate` only advertises default route if that router has a static default route. otherwise no default route is advertised.
