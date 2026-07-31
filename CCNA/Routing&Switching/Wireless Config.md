# Wireless Config

![[Wireless Scenario.excalidraw|800]]

## WLAN Initial Config

```
   Run WLAN:
	1. give system name
	2. give admin user&pass
	3. create LAG
	4. give mgmt ip
	5. give mgmt VLAN
	6. give mgmt port
	7. give DHCP server IP 
	8. virtual gateway ip -> 192.0.2.1
	9. give multicast ip -> 239.x.x.x
	10. give RF group name
	11. give SSID name
	12. enable DHCP bridging
	13. allow static ip 
	14. configure RADIUS server
	15. give country code
	16. enable wireless version
	17. enable auto RF
	18. configure NTP
	19. reload
   
   in WLC CLI:
	show sysinfo
	config wlan create <WLAN ID> <WLAN Name> <SSID>
	config wlan apgroup add <AP Group Name>

   3750 Switch:
	conf t
	int ra gig 1/0/1-2
		sw mo trunk
		channel-group 1 mode on
		exit
	int gig 1/0/3
		sw mo acc
		sw acc vlan 1
		exit
	int gig 1/0/4
		sw mo acc
		sw acc vlan 10
   
   WLAN Web Ui config:
	in web ui -> WLANs -> create new & edit it
	controller -> interfaces -> create new -> give IP & VLAN ID
	WLANs -> advanced -> AP Groupes -> add group
   
   AP Connection:
	capwap ap primary-base <WLC Name> <WLC IP>  \\ if we configure it manually
```

## WLAN Configuration

```
   in WLC Web UI:
	WLANs -> create
	give SSID, ID, profile name, type -> apply
	choose general and security settings -> apply
	// create one for Guest SSID and one for Staff SSID
	Wireless -> 802.11 -> enabled
```

## WPA2 Enterprise Configuration

```
   RADIUS Server:
	in WLC Web ui -> Security -> AAA -> RADIUS -> Authentication -> new
	give ip, shared secret, port, server status 
	in WLAN edit -> security -> L2
	L2 Sec -> WPA+WPA2
	enable 802.1x
	enable WPA2 policy
	apply
```

## QoS

```
   in WLC Web ui:
	WLAN -> edit -> QoS
	choose QoS for current SSID
```
