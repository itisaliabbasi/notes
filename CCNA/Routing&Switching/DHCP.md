# DHCP

 > *DHCP => Dynamic Host Configuration Protocol*
> *DC => Domain Controller*

## DHCP Process

> *DORA => Discover, Offer, Request, ACK*
> 1. Discover: broadcast for DHCP server
> 2. Offer: DHCP server offers an ip to host (unicast)
> 3. Request: host requests ip to DHCP (unicast)
> 4. ACK: DHCP server leases ip for host (unicast)

> [!NOTE]
> Discover is a broadcast traffic in a broadcast domain and if DHCP server is on another broadcast domain (another VLAN or 1 or more hops away), our hosts are unable to see DHCP server. in this case we should use DHCP relay.

## DHCP Config

```
conf t
int gig 0/1
	ip add 192.168.1.1 255.255.255.0
	exit
ip dhcp pool <pool_name>
	network <net_ID> <net_mask>
	default-router <gateway_IP>
	exit
ip dhcp excluded-address <IP> //globally exclude ip from dhcp pools
```

## DHCP Relay Config

```
on a router that can see both discover messages and DHCP server:
conf t
int gig 1/0
	ip helper-address <DHCP_server_IP>
```

## DHCP Snooping

```
conf t
ip dhcp snooping
ip dhcp snooping vlan <id,id,...>
no ip dhcp snooping information option //doesnt use opiton 82, if you configure this option on your DHCP server, its ok not to use this command
interface gig 0/1 //server faced interface
	ip dhcp snooping trust
	ip dhcp snooping limit rate 50
ip dhcp snooping database <path-to-database-file> //imports a dhcp snooping database from an external file
show ip dhcp snooping database
show ip dhcp snooping
show ip dhcp snooping binding
```

## DAI

> *DAI => Dynamic ARP Inspection* -> checks (with dhcp snooping database) that a client cant have same ip with 2 different MAC addresses.

```
show ip arp inspection interfaces
conf t
ip arp inspection vlan <id,id,...>
sh ip arp inspection
sh ip arp inspection statistics vlan <id>
int gig 0/0 //on trunk ports
	ip arp inspection trust
	exit
```
