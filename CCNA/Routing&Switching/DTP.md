# DTP

> *DTP => Dynamic Trunking Protocol*
> negotiates trunking between 2 switch or routers

```
int gig 1/0/1
switchport mode dynamic auto/desirable //to configure DTP
switchport nonegotiate //to Disable DTP
sw mode trunk
sw trunk native vlan <id>
```

> [!NOTE]
> In order to prevent VLAN Hopping Attack we must use `sw nonegotiate` on trunk ports and `sw mode access` on access ports and also change native VLAN of trunk port to anything other than VLAN 1.

## DTP Trunking Table

|           | Auto | Desirable | Trunk |
| :-------: | :--: | :-------: | :---: |
|   Auto    |  no  |    yes    |  yes  |
| Desirable | yes  |    yes    |  yes  |
|   Trunk   | yes  |    yes    |  yes  |
