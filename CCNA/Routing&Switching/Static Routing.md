# Static Routing

> `sh ip route` shows all the routes on a router or MLS

## Route Types

1. Static network route:
	`ip route 10.10.10.0 255.255.255.0 192.168.1.1`
2. Static host route:
	`ip route 10.10.10.34 255.255.255.255 192.168.1.1`
3. Default route:
	`ip route 0.0.0.0 0.0.0.0 192.168.1.1`

## Administrative Distance

> *AD => Administrative Distance*
> RIP default AD is 120
> OSPF default AD is 110
> EIGRP default AD is 120
> Directly Connected routes default AD is 0
> eBGP default AD is 20
> iBGP default AD is 200
> Static routes default AD is 1

## Floating Static Route

> Having a backup route to a destination beside our primary route. so if primary goes down the backup route can work. it also works on Default Routes.

```
ip route 192.168.1.0 255.255.255.0 10.10.10.1
ip route 192.168.1.0 255.255.255.0 10.10.11.1 2
```

## Forwarding Decisions

> 1. *LPM => Longest Prefix Match*
> 2. AD
> 3. Metric or Cost

> [!NOTE]
> If there are multiple routes to a destination but each one has a different subnet mask, the route with longest subnet bits match (LPM) wins despite having even higher AD.(for example if there is a /25 and a /24 route to 192.168.0.0 destination, /25 route wins)

> *ECMP => Equal Cost Multi-Path*
> ECMP is where multiple routes have same AD, prefix and cost. now between them load balance happens.
> *CEF => Cisco Express Forwarding* -> routes are processed and cached, so with arrival of packet it will be forwarded immediately

## Best Path Calculation

1. OSPF: cost
2. RIP: hop count
3. EIGRP: metric
