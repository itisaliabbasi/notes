# Port Security

> Violation modes:
> 1. Protect: doesnt let computer with unauthorized MAC to connect to network and doesnt log the violation.
> 2. Restrict: doesnt let computer with unauthorized MAC to connect to network and logs the violation.
> 3. shutdown: turn off the port if unauthorized MAC is seen on it.

> [!NOTE]
> We cannot run PortSec on an interface with DTP.

## PortSec MAC Learning

1. Dynamic: dynamically learn MACs on interface
2. Static: manually set MAC on an interface
3. Sticky: dyanmically learn MACs on interface but doesnt let the same MAC connect to another port

## PortSec Config

```
//on Access ports
conf t
int ra gig 0/1-3
	sw port-security max 3
	sw port-security mac <sticky|MAC-address>
	sw port-security violation restrict
	sw port-security aging time <number-minutes>
	sw port-security aging type <inactivity|absolute>
	sw port-security
show port-security
show port-security address
show interface err-disabled
```

> [!NOTE]
> VIP of an FHRP has its own MAC address so keep it in mind to increase PortSec max MAC by 1 on interface that sees FHRP VIP and make  it dynamic. or use `standby use-bia` config for HSRP to use physical MAC for VIP.
