# Network Design

## Modular Network

![[ModularNetwork.excalidraw|800]]

## 3-tier Network Design

![[3-tierNetworkDesign.excalidraw|800]]

> [!NOTE]
> In some smaller networks we dont need a 3-Tier network, we can collapse multiple tiers into one, for example we can collapse Distribute and access or distribute and core or distribute and access and core altogether. its called collapsed architecture.

> [!NOTE]
> Each Dist block only can have 2 switches that must be connected to each other. if we want to add to it, it must be a second distribution block. (no East-West Traffic)
> Between Distribution blocks no connectivity is allowed. each one of these blocks must connect to core and dist blocks can have traffic between them through Core Block.(North-South Traffic)

### Core Requirements

1. all connections are L3 routed (no VLAN, no STP)
2. if its multicore design, there must be full mesh between each pair of core switches
3. 2 interconnected devices
4. main concern is speed (no QoS, no NAT, no ACL)
5. all core devices must operate as Active/Active

## Access Layer

1. L2: it needs configuring STP to prevent loops and fully use all links with PVST.

![[Screenshot From 2026-08-27 16-51-45.png]]

2. L2 Loopfree

![[Screenshot From 2026-08-27 16-51-14.png]]

3. L3: its loopfree and we dont need to worry about STP, but costs more and we cant have same VLAN with same subnet on multiple switches.

![[Screenshot From 2026-08-27 16-50-24.png]]

### Topologies

1. VSS and Stackwise/ChassisStackable: latest tech, more cost, best efficiency and redundancy
2. Daisy chain: low efficiency
3. Redundant connections: high cost
4. Efficient connections: no redundancy

![[AccesslayerTopologies.excalidraw|800]]

> *VSS => Virtual Switching System*

## Datacenter Topologies

1. Agg+Acc Layers: L2 Connection towards Access tier, low scalability, legacy.
2. Spine-Leaf: all L3 connection, Spine tier only does fast L3 switching, all other modules are connected to Leaf nodes, modern.

![[DatacenterTopologies.excalidraw|800]]

## Legacy Network Challenges

1. Applying consistent policy for config standards
2. Applying consistent security policies across all network users, regardless of mobility
3. Locating & troubleshooting user issues

> Fabric architecture solves all these problems.

## Fabric Architecture

1. everyting (underlay network) is L3
2. overlay network will have fabric: control and data planes are centrally controlled(SDN)
	1. SD-Access: user/device segmentation inside a site, providing unified wired and wireless access
		1. LISP
		2. VxLAN
		3. automated using cisco catalyst center(cisco DNA center)
![[Screenshot From 2026-08-27 19-32-12.png]]
	2. SD-WAN: path control & performance between sites
![[Screenshot From 2026-08-27 19-39-13.png]]
![[Screenshot From 2026-08-27 19-39-22.png]]
