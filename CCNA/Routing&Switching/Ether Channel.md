# Ether Channel

## Port Types

1. Access (L2): `switchport mode access`
2. Trunk (L2): `switchport mode trunk`
3. Routed (L3): `no switchport`

## Ether Channel Protocols

> 1. *PAgP => Port Aggregation Protocol* -> Cisco Propriatary:
> 	1. Desirable
> 	2. Auto
> 2. *LACP => Link Aggregation Control Protocol* -> Open Standard:
> 	1. Active
> 	2. Passive
> 3. ON: static, no protocol is used

> Each port group interface configured with LACP is called LAG.
> *LAG => Link Aggregation Group*

### Etherchannel Connection

|   PAgP Mode   | Auto | Desirable | ON  |
| :-----------: | :--: | :-------: | :-: |
|   **Auto**    |  no  |    yes    | no  |
| **Desirable** | yes  |    yes    | no  |
|    **ON**     |  no  |    no     | yes |

| LACP Mode | Passive | Active | ON  |
| :-------: | :-----: | :----: | :-: |
|  **Passive**  |   no    |  yes   | no  |
|  **Active**   |   yes   |  yes   | no  |
|    **ON**     |   no    |   no   | yes |

## Ether Channel Config

![[Ether Channel.excalidraw|800]]

```
SW-2:
conf t
int ra gig 1/0/1-4
sw mo tru
sw trunk encapsulation dot1q
channel-group 1 mode [auto|desirable]
exit
int port-channel 1
sw mo trunk
sw tru enc dot1q
exit
etherchannel load-balance [algorithm]
exit
sh etherchannel load-balance
sh etherchannel summary
sh int status
sh etherchannel detail
sh int tru

SW-1:
conf t
int ra gig 1/0/1-4
sw mo tru
sw tru enc dot1q
channel-group 1 mode [auto|desirable]
exit
int port-channel 1
sw mo trunk
sw tru enc dot1q
exit
int ra gig 1/0/5-6
sw mo tru
sw tru enc dot1q
channel-group 2 mode [active|passive]
exit
int po 2
sw mo tru
sw tru enc dot1q
exit
etherchannel load-balance [algorithm]
exit
sh etherchannel load-balance
sh etherchannel summary
sh int status
sh etherchannel detail
sh int tru

SRV:
\\ Config the interfaces with same mode and protocol as switch (ON|LACP) and based on server config we can configure it as trunk or access or routed interface on switch
```
