# VTP

> *VTP => VLAN Trunking Protocol*

## VTP Components

1. VTP Domain
2. VTP Password
3. VTP Version:
	1. v1
	2. v2
	3. v3
4. VTP Mode:
	1. Client: cant create VLAN, only gets from server
	2. Server: create VLAN, gives it to other switches if has highest revision number
	3. Transparent: doesnt participate in VTP, but relays VTP messages
	4. Off
5. Config Revision: swtich with highest revision number does the vtp job between all switches with server mode

> [!NOTE]
> To set revision number of a swtich to 0, we should put its VTP to transparent mode and then set it back to client or server mode.

## VTP Config

![[VTP Scenario.excalidraw|800]]

```
// Trunk all links between switches
conf t
vtp domain [domain-name]
vtp password [pass]
vtp mode [mode]
// Create VLAN on any VTP Server SW
show vtp status
show vtp counters
```
