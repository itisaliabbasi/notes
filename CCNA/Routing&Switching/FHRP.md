# FHRP

> *FHRP => First Hop Redundancy Protocol*
> 1. *HSRP => Hot Standby Router Protocol*
> 2. *VRRP => Virtual Router Redundancy Protocol*
> 3. *GLBP => Gateway Load Balance Protocol*

## HSRP Config

```
on R1:
	conf t
	int gig 0/0
	standby <group_number> ip <VIP>
	exit
	standby <group_number> preempt
	show standby

on R2:
	conf t
	int gig 0/0
	standby <group_number> ip <VIP>
	exit
	standby <group_number> preempt
	show standby

// on clients set VIP as gateway
```

## VRRP Config

```
on R1:
	conf t
	int gig 0/0
	vrrp <group_number> ip <VIP>
	exit
	vrrp <group_number> priority <priority_number>
	show vrrp

on R2:
	conf t
	int gig 0/0
	vrrp <group_number> ip <VIP>
	exit
	vrrp <group_number> priority <priority_number>
	show vrrp

// on clients set VIP as gateway
```

> [!NOTE]
> VRRP has preemption enabled by default.
