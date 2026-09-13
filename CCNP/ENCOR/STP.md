# STP

> *STP => Spanning Tree Protocol*
> *RSTP => Rapind STP* -> 802.1d & 802.1w
> *MST => Multi Spanning Tree*
> *PVST => Per VLAN Spanning Tree*
> *RPVST => Rapid PVST*

![[Screenshot From 2026-08-28 20-00-05.png|800]]

> `show spanning-tree summary` shows the status of our spanning tree
> `spanning-tree vlan <id> hello-time <sec>` to change hello timer for a switch in vlan
> `debug spanning-tree event` show what is happening in STP live.

![[Screenshot From 2026-09-07 18-55-18.png|800]]

> If lots of our links are half duplex, there is lots of Hubs, then RSTP wont work correctly.

![[Screenshot From 2026-09-07 20-42-16.png|800]]
![[Screenshot From 2026-09-09 16-34-15.png|800]]

> default RSTP port priority is 128 and other ports that connect to same switch will have lower priority.

![[Screenshot From 2026-09-09 16-38-35.png|800]]

## RSTP Topology Change

> *TCN => Topology Change Notification* -> BPDU that lets other nodes know that a change happened in topology
> *BPDU => Bridge Protocol Data Unit*

![[Screenshot From 2026-09-09 16-44-56.png|800]]

> when a change occures TC-Flag sets MAC address-table aging from 300 sec to 15 (forwarding delay time) seconds.
> non-edge ports (`no spanning-tree portfast`) become forwarding.

![[Screenshot From 2026-09-09 16-50-18.png|800]]
![[Screenshot From 2026-09-09 17-01-15.png|800]]

## MST

> doesnt run individual instances for each VLAN, it says how many paths are available? (eg 3), then runs (eg 3) instances for each path and groups (eg 3) some VLANs together and sends a group in each path. # of VLANs in each group is irrelevant to other group. this process is called *decupling* VLANs from instances.
> running PVST consumes alot of switch CPU. for example if switch has max number of 64 instances of PVST, means the 65th VLAN doesnt have a PVST instance running for it. and if we have (eg 60) VLANs the CPU usage will go high. for high scale networks that the # of VLANs is very high, we must use MST instead of PVST.

![[Screenshot From 2026-09-10 13-21-43.png|800]]
![[Screenshot From 2026-09-10 13-21-56.png|800]]
![[Screenshot From 2026-09-10 13-30-22.png|800]]
![[Screenshot From 2026-09-10 13-32-14.png|800]]
![[Screenshot From 2026-09-10 13-33-58.png|800]]
![[Screenshot From 2026-09-10 13-34-39.png|800]]

### PVST VS MST

![[Screenshot From 2026-09-10 13-35-23.png|800]]
![[Screenshot From 2026-09-10 13-36-12.png|800]]
![[Screenshot From 2026-09-10 13-44-41.png|800]]
![[Screenshot From 2026-09-10 13-51-21.png|800]]

### MST Tuning

![[Screenshot From 2026-09-12 20-05-37.png|800]]
![[Screenshot From 2026-09-12 20-07-41.png|800]]

### MST Boundary Port

![[Screenshot From 2026-09-12 20-10-56.png|800]]
![[Screenshot From 2026-09-12 20-12-30.png|800]]
![[Screenshot From 2026-09-12 20-15-12.png|800]]

### MST Virtual Bridge

![[Screenshot From 2026-09-12 20-22-46.png|800]]
![[Screenshot From 2026-09-12 20-27-15.png|800]]
![[Screenshot From 2026-09-12 20-30-16.png|800]]

### Master Root Port Selection

![[Screenshot From 2026-09-13 19-09-57.png|800]]
![[Screenshot From 2026-09-13 19-12-38.png|800]]

### MST PVST Simulation

![[Screenshot From 2026-09-13 19-16-52.png|800]]
![[Screenshot From 2026-09-13 19-18-26.png|800]]
![[Screenshot From 2026-09-13 19-20-21.png|800]]
![[Screenshot From 2026-09-13 19-22-43.png|800]]
![[Screenshot From 2026-09-13 19-58-03.png|800]]
![[Screenshot From 2026-09-13 19-59-57.png|800]]
![[Screenshot From 2026-09-13 20-01-40.png|800]]
