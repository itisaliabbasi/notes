# Design

## Topologies

1. Serial
2. Star
3. Full Mesh
4. Partial Mesh

## Modular Hierarchy

1. Access
2. Core
3. Aggregation
4. Distribution
5. WAN
6. Internet (Edge)

> [!Note]
> There can be multiple modules of same type in a network.

> Traditionally there was 3 layered campus design (Core, Distribute, Access), but now its replaced by modular design. between core and distribute modules we have full mesh and between distribute and access we have partial mesh.

> Access block for users must be connected to distribution block and aggregation is for data centers so we have an access block connected to it for datacenter.

![[Modular Hierarchy.excalidraw|800]]

> *SVI => Switched Virtual Interface* -> VLAN Interface
> *FHRP => First Hop Redundancy*

### FHRPs

> 1. *VRRP => Virtual Router Redundancy* -> open standard
> 2. *GLBP => Gateway Load Balance Protocol* -> Cisco Propriatary
> 3. *HSRP => Hot Standby Router Protocol* -> Cisco Propriatary

> [!NOTE]
> Instead of Agg and Acc blocks for Datacenters, we can have Spine and Leaf architecture. its all based on L3 connections and routing protocols such as iBGP & eBGP.

![[Spine&Leaf.excalidraw|800]]

> *WAN => Wide Area Network
> BGP => Border Gateway Protocol*

> [!NOTE]
> In smaller scales of networks we can combine multiple blocks, its called *Collapsed Architecture*.

## Network Traffic

1. North-South: flows in Acc and Dist/Agg blocks -> user traffic goes up to Dist and analized and comes down to Acc
2. East-West: flows in Spine and Leaf Architecture -> traffic between servers, doesnt go up, its analized in Spine switches

### Fiber

1. Lit Fiber: service provider does the work -> shared connection, so we wont get full BW of link
2. Dark Fiber: we do the work ourselves -> we implement infrastructure, so we bet full BW of link

### Wireless Bridging

> 1. *PtP => Point to Point*
> 2. *PtMP => Point to Multi-Point*

## Configure Interface

### Speed & Duplex

```
conf t
interface gig 1/0/1
speed 1000
duplex full
no shut
end
show interface gig 1/0/1
```

### Line Protocol (L2)

```
conf t
interface gig 1/0/1
encapsulation <protocol>
```

## Interface Status

1. UP/UP: L1/L2 is ok
2. UP/Down: L2 not ok -> line protocols are not same on both sides
3. Administratively Down/Down: L1 shut down manually
4. Down/Down: not connected, speed mismatch
5. ERR-Disabled: interface flapping, port-security violation

> [!important]
> When there is Duplex mismatch, interface status wont go down (its UP/UP), but error log is shown rapidly and connection is unstable.
> also number of collisions on that interface goes up, so we can check it in `show int gig 1/0/1`

> [!tip]
> In case of ERR-Disabled, we should shut, no shut the interface.

> MTU => Maximum Transmission Unit

`system mtu [x]` lets us to change MTU of frames on all interfaces.

### IPv4

```
conf t
int gig 1/0/1
ip add 192.168.1.1 255.255.255.0
no sh
end
sh ip int bri
ping [ip]
```

#### IPv4 Classes

| Class | First Octet of IP |  /  |     Mask      |
| :---: | :---------------: | :-: | :-----------: |
|   A   |       0-127       | /8  |   255.0.0.0   |
|   B   |     128 - 191     | /16 |  255.255.0.0  |
|   C   |     192 - 223     | /24 | 255.255.255.0 |

> *NAT => Network Address Translation*

> [!NOTE]
> `show history` shows a history of recent commands we have used.

> *VLSM => Variable Length Subnet Mask*

#### IPv4 Address Types

 1. Unicast: one to one Dst -> `192.168.1.1 - 192.168.1.254`
 2. Broadcast: one to all hosts in broadcast domain, its the last ip in a subnet -> `192.168.1.255` for example

> [!NOTE]
> Broadcast can be in both L2 or L3 addresses. in L2 its FF:FF:FF:FF:FF:FF, in L3 it can be last ip of each subnet or 255.255.255.255 in general.

 3. Multicast: one to multiple Dst's -> `224-239.0.0.0` ip, in OSPF its `224.0.0.5`, in RIP its `224.0.0.9`

> [!NOTE]
> So we must be carefull because some protocols have their own multicast ip address and we should not use that ip.

 4. Anycast: whoever gets to it first
 5. Link Local: when DHCP server is not responding (*APIPA => Automatic Private IP Addressing*) -> `169.254.0.0` ip range
 6. Loopback: inside virtual ip (eg. 127.0.0.1), used for testings -> `127.0.0.0` ip range

> *IGMP => Internet Group Management Protocol*

#### Anycast Config

```
conf t
int loo 1
ip address 77.77.77.77 255.255.255.0
exit
sh ip int bri
sh ip route
// Then run any routing protocol, automaticaly when a host wants to reach that ip, nearest one will reply.
```

#### Multicast Config

```
conf t
int gig 1/0/1
ip igmp join-group 224.0.0.1
exit
sh ip igmp groups
```

### IPv6

> [!NOTE]
> Its shown with Hex numbers(0-9,A,B,C,D,E,F), (IPv4 is shown with decimal) and its 128 bit long:
> `1234:1234:1234:1234:1234:1234:1234:1234`

> *SLAAC => State Less Address Auto Configuration*
> *NDP => Network Discovery Protocol*
> IPv6 has all IPv4 features, and it has SLAAC & NDP that IPv4 doesnt have.
> NDP is like ICPM but for IPv6.

#### IPv6 Summerization

`2001:0DB8:6783:000A:0000:0000:0000:0010` => `2001:DB8:6783:A::10`
`2001:0000:0000:00A4:0000:0000:0000:0005` => `2001:0000:0000:A4::5`

#### IPv6 Address Types

| Address     | Purpose              | Example                       |
| ----------- | -------------------- | ----------------------------- |
| 2000-3FFF:: | Global Unicast       | 2001:DB8:6743:A::1/64         |
| FC00::/7    | Unique Local Unicast | FC00:1234:ABDC:BAD:F00D::9/64 |
| ::1/128     | Loopback             | ::1/128                       |
| FE80::/10   | Link Local           | FE80::12:34:56::99:22         |
| FF00::/8    | Multicast            | FF02::9                       |

> [!NOTE]
> EUI-64 is a feature in IPv6 that automaticaly generates ip based on interface MAC address in /64 subnet of IPv6.

```
conf t
int gig 1/0/1
ipv6 add 2001:db8:6783:a::/64 eui-64
ipv6 add fe80::1 link-local
exit
show ipv6 interface gig 1/0/1	
```

> *DAD => Duplicate Address Discovery*

> [!NOTE]
> If we configure IPv6 address on an interface multiple times, all of the addresses will be on that interface (doesnt need secondary in its command, unlike IPv4), so be carefull when configuring it.

#### Multicast Config

```
conf t
no ipv6 unicast-routing
do sh ipv6 int gig 1/0/1
ipv6 unicast-routing
do sh ipv6 int gig 1/0/1
```

> We can also configure IPv6 on an interface with `ipv6 address autoconfig` command on interface

## Virtualization

> *NFV => Network Function Virtualization* -> virtualizing all network devices
> *VNF => Virtual Network Function* -> an instance of a virtual network device

### Cisco Virtual Appliances:

1. CSRv -> router
2. 8000v -> router
3. FTDv -> firewall
4. ASAv -> firewall
5. NGFWv -> firewall
6. vWLC
