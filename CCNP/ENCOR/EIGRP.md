# EIGRP

## Prerequisites of EIGRP

1. Routers be on same AS Number
2. Neighboring routers must be on same subnet
3. Matching key values (for Classic EIGRP)
	1. Bandwidth
	2. Delay
	3. Reliability
	4. Load
	5. MTU
4. Hello Timer
5. Auth

## Named VS Classic EIGRP

1. Named supports both ipv4 and ipv6 in an instance while Classic only can have one address family in each instance
2. Named is 64 but Classic is 32 bits
3. Named is better when we have a wide range of interface speeds
4. Named has 6 Key Values, 5 same as Classic and the sixth one is Jitter
5. Named is backwards compatible to Classic
6. Named supports MD5 and HMAC-SHA for auth while Classic only supports MD5
7. Named Delay is in Pico Seconds while in Classic its Micro Seconds

## Classic Config

```
conf t
	router eigrp <AS-Number>
		network <Net-ID> <Wild-Card>
		network 0.0.0.0 255.255.255.255 //any interface with ip
		network <ip-address> 0.0.0.0 //only an interface with this specific ip
		exit
	exit
sh ip eigrp neighbors
sh ip eigrp
sh ip protocols
sh ip route eigrp
sh ip eigrp topology <int-ip-address> 255.255.255.255
logging console
ter mon
debug eigrp message 
debug eigrp packets
```

> [!NOTE]
> EIGRP does ECMP load balancing for 4 paths. it can be configured for up to 32 paths.

![[screenshot_2026-09-18_21-49-05.png|800]]

> M => Metric
> B => Bandwidth
> D => Delay

> [!NOTE]
> Some commands happen under interface and some commands are under protocl. but in Named EIGRP everything is under protocol.

## Named Config

```
conf t
	router eigrp <name>
		address-family <ipv4|ipv6> autonomous-system <AS-Number>
		network <Net-ID> <Wild-card>
		network 0.0.0.0 255.255.255.255 //any interface with ip
		network <ip-address> 0.0.0.0 //only an interface with this specific ip
		exit
	exit
sh ip eigrp neighbors
sh ip eigrp
sh ip protocols
sh ip route eigrp
sh ip eigrp topology <int-ip-address> 255.255.255.255
logging console
ter mon
debug eigrp message 
debug eigrp packets
```

> [!NOTE]
> Under Classic EIGRP protocol we can migrate to Named with `eigrp upgrade-cli <name>`

## EIGRP Auth

### Classic

```
conf t
	key chain <name>
		key <id>
		key-string <password>
		send life-time <start-time> <end-time> //use this if you have multiple keys and want key rotation
		exit
	exit
	int fa 0/0 //neighboring int to other router
		ip authentication mode eigrp <ASN> <md5>
		ip authentication key-chain eigrp <ASN> <keychain-name>
		exit
	exit
```

> [!NOTE]
> Each keychain can have multiple keys,for key rotation and we can put timers for each key id so it expires and rotates to next key id.

> [!NOTE]
> If a router has multiple hands in EIGRP, we must create a seperate keychain for each hand.

### Named

```
conf t
	router eigrp <name>
		address-family ipv4 autonomous-system <ASN>
			af-interface gig 0/0
				autentication mode hmac-sha-256 <password>
```

## Offset-list

> Means changing the metric of routes that we send/recieve to/from our neighbors.
> Using access-list 0 means match all prefixes.
> The access list must be permit prefix of the routes that we want to change their metric with offset-list.

```
conf t
	router eigrp <name|number>
		address-family ipv4 autonomous-system <ASN> //on named EIGRP
			topology base //on named EIGRP
				offset-list <ACL-Number|Name> <in|out> <add-#-to-original-metric> <interface>
```

## AD

1. Internal EIGRP: 90
2. External EIGRP: 170 -> redistributed routes
3. Local Null Zero Route: 5 -> is created due to summerization on the router that does summerization (on other routers its 90)

> `redistribute connected <metric>` to redistribute connected routes into EIGRP.

> [!NOTE]
> Path manuplation can be done with changing AD or Metrics, its up to us and the situation to choose which one. on both of them it must be done using ACL. (external EIGRP routes AD cant be changed with ACL)
> - AD is better for when we have multiple protocols
> - Metric is for when we have a protocol

```
change AD globally for EIGRP:
conf t
	router eigrp <name|number>
		address-family ipv4 autonomous-system <ASN> //on named EIGRP
			topology base //on named EIGRP
				distance eigrp <internal-AD> <external-AD>
```

> [!NOTE]
> AD is never advertized and we must configure and change it on that router. while Metric can be advertized using offset lists.

### Routing Path Selection

1. longest prefix match
2. AD
3. Metric
4. ECMP/UCMP

```
change AD of specified routes learned from a neighbor:
conf t
	router eigrp <name>
		address-family ipv4 autonomous-system <ASN>
			topology base
				distance <AD> <neighbor-ip> <wildcard-mask> <ACL>
```

## Summerization

> [!NOTE]
> In EIGRP, summerization can happen on any router in topology.

```
conf t
	router eigrp <name|number>
		address-family ipv4 autonomous-system <ASN> //on named EIGRP
			af-interface <int-number>
				summary-address <net-id>/<subnet-mask>
sh ip route eigrp //AD must be 5
```

## ECMP/UCMP

> *FD => Feasible Distance* -> sum of metrics of hops from a src to a dst
 > Successor is the path with lowest FD from a src to dst. feasible successor is the bakcup path with second lowest FD a backup path that if the main one goes down this one works.
 > Feasible successor path RD must be less that successor path FD so it can be chosen as feasible successor.
 > Only the successor path goes into routing table. but we can see feasible successor path data in topology table. (`sh ip eigrp topology <dst-ip> <subnet-mask>`)
 > *RD => Reported Distance* -> Advertised Distance(RD), is the metric that our neighboring router is advertising to us.

![[screenshot_2026-09-21_22-03-06.png|800]]

> [!NOTE]
> Its because of successor and feasible successor path that EIGRP has such a low convergence time. becase after the successor path goes down, feasible successor path immediately becomes successor path.

> changing key values: `metric weighs <k1> <k2> <k3> <k4> <k5> <k6>` that we specify if the key value is used in metric calculation (1) or not (0). this must be the same across all the routers in topology. this config is under router eigrp (if its named, we should do it under ipv4 address family).

> *ECMP => Equal Cost Multi Path* -> happens when FD of multiple paths are the same (by default up to 4 paths, but can be configured to up to 32 paths)
> *UCMP => Unequal Cost Multi Path* -> happens when FD of feasible successor is <= FD of successor * variant
> To configure variant we shuld run this command under router eigrp`variant <x>`.
> In UCMP, the feasible successor path is also in routing table.

![[screenshot_2026-09-21_22-35-57.png|800]]

## EIGRP Stub

> Used to reduce amount of queries in EIGRP topology. always must be configured on dead end and spoke routers and makes them not recieve any query. (not hub routers)
> A query is sent when successor path goes down and we have no feasible successor.

![[screenshot_2026-09-22_11-49-05.png|800]]

![[screenshot_2026-09-22_11-50-38.png|800]]

```
conf t
	router eigrp <name|number>
		address-family ipv4 autonomous-system <ASN> //on named EIGRP
			eigrp stub
sh ip route eigrp //AD must be 5
```

> [!NOTE]
> - if a hub router becomes stub, the routes of routers behind it will be seen on that but they cant be advertized because stub doesnt recieve any queries, so a part of network will not be available.
> - in some cases we can have a hub router as stub, but we must utilize summerizing so it can advertize the routes behind it.
> - also some times we can leak routes from stub router with ACL & route maps `eigrp stub connected summary leak-map <route-map>`.
