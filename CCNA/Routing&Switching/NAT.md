# NAT

> *NAT => Network Address Translation*
> 1. Destination NAT: destination ip is translated
> 2. Source NAT: source ip is translated
> 3. Static NAT: 1 to 1
> 4. Dynamic NAT: many hosts are translated to ip of an interface that we dont know (1 to 1)
> 5. *PAT => Port Address Translation*:  many hosts are translated to a single ip on interface (overload) and each translation is distinguished by the port they use (masquerade)

## NAT Address Types

1. Inside/Outside: where the host is
2. Local/Global: who is viewing that host

![[NAT.excalidraw|800]]

|         | Local       | Global   |
| ------- | ----------- | -------- |
| Inside  | 192.168.1.1 | 25.1.1.1 |
| Outside | 8.8.8.8     | 8.8.8.8  |

## Static NAT Config

```
conf t
int gig 0/0
	ip nat inside
	exit
int gig 0/1
	ip nat outside
	exit
ip nat inside source static <local_IP> <global_IP>
show ip nat translations
```

## Dynamic NAT Config

```
conf t
int gig 0/0
	ip nat inside
	exit
int gig 0/1
	ip nat outside
	exit
ip nat pool <pool_name> <IP_range> netmask <subnet>
ip access-list extended <name>
	//permit or deny rules
ip nat inside source list <access_list_name> pool <pool_name> 
```

> [!NOTE]
> in dynamic nat, source list is an access list that tells which local addresses can be nated or not and pool has the list of ips on outside interface of router.

## PAT Config

```
conf t
int gig 0/0
	ip nat inside
	exit
int gig 0/1
	ip nat outside
	exit
ip access-list extended <name>
	//permit or deny rules
ip nat inside source list <access_list_name> interface g0/1 overload
```
