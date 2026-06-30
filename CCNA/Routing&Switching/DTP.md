# DTP

> *DTP => Dynamic Trunking Protocol*
> negotiates trunking between 2 switch or routers

```
int gig 1/0/1
switchport mode dynamic auto/desirable //to configure DTP
switchport nonegotiate //to Disable DTP
```

## DTP Trunking Table

|           | Auto | Desirable | Trunk |
| :-------: | :--: | :-------: | :---: |
|   Auto    |  no  |    yes    |  yes  |
| Desirable | yes  |    yes    |  yes  |
|   Trunk   | yes  |    yes    |  yes  |
