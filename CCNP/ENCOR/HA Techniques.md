# HA Techniques

## FHRP

1. HSRP: Higher priority wins, hello message each 3sec, hold timer 10sec, can have auth(text, MD5), has active/standby, cisco proprietary.
	1. v1: 256 Groups, vMAC(`0000.0c07.ac**`), multicast(`224.0.0.2`)
	2. v2: 4096 Groups, its timers can go below 1 sec, vMAC(`0000.0c9f.f***`), multicast(`224.0.0.102`)

> [!NOTE]
> In datacenters when we configure HSRP and VPC together on nexux switches, both active and standby switches process packets(data plane) at the same time(because of loadbalance that VPC does).

2. VRRP: has master/backup, open standard, can have auth(text, MD5), vMAC(`0000.5e00.01**`), multicast(`224.0.0.18`)
	1. v2: used in IPv4
	2. v3: required for IPv6

> [!NOTE]
> in VRRP, master router physical IP can be the vIP of VRRP(unlike HSRP).

3. GLBP: the router with top priority becomes AVG and answers ARP request, if AVG goes down another router with highest priority becomes AVG, and all other routers become AVF and each one has its own vMAC, if an AVF go down, its vMAC is assigned to another router. loadbalance is done with 3 algorithms(Round Robin, Weighted, Host Dependent), vMAC(`0007.b40*.**!!`), has 1024 Groups, multicast(`224.0.0.102`) same as HSRP v2.

> [!NOTE]
> HSRP/VRRP are used for 2 routers/MLSs, while GLBP can be used for up to 4 routers/MLSs.

> [!NOTE]
> in FHRP vMACs * is for group ID, in GLBP vMAC, !! is (01-04) for showing AVF.

> *VPC => Virtual Port Channel*
> *AVG => Active Virtual Gateway*
> *AVF => Active Virtual Forwarder*
> in VRRP, Preemption is enabled by default, but in HSRP its disabled.

### FHRP Design

> its better to have STP root as primary FHRP router or also VPC primary.
> its better to run FHRP for each VLAN and choose a different router as primary for each VLAN in conjunction with PVST to loadbalance.

## SSO and NSF

> *SSO => Statefull Switch Over*
> *NSF => Non-Stop Forwarding*
> a chassis device can have 2 supervisor chip, 2 route processor modules, 2 control planes and 2 data planes. one contol plane is supervisor and copies all its data to other control plane but only one data plane is active, so when our primary data plane and control plane goes down, the other ones come up in seconds and do the work. this process is SSO. if running route processor fails, router adjancies go down but if it supports NSF, routers go in grace mode and routes become stale. the other routers send route data (only routing table not routing protocol data) on data plane in hope that the router will be up in a few seconds, then backup route processor comes up and send NSF data to other routers and routing protocols data will be sent and routes wont be marked stale anymore.

> [!NOTE]
> SSO is for keeping data plane up. but NFS keeps router adjancies up so forwaring continues until route processor comes up.
