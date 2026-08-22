# Network Design

## Modular Network

![[ModularNetwork.excalidraw|800]]

## 3-tier Network Design

![[3-tierNetworkDesign.excalidraw|800]]

> [!NOTE]
> In some smaller networks we dont need a 3-Tier network, we can collapse multiple tiers into one, for example we can collapse Distribute and access or distribute and core or distribute and access and core altogether. its called collapsed architecture.

## Access Layer

1. L2: it needs configuring STP to prevent loops and fully use all links with PVST.
2. L3: its loopfree and we dont need to worry about STP, but costs more and we cant have same VLAN with same subnet on multiple switches.

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
