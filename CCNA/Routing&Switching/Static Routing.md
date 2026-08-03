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
