# Wireless

> *RF => Radio Frequency*
> *AP => Access Point*

## WIFI Frequencies (WIFI Bands)

1. 2.4GHz, 22MHz Channels -> 2.4-2.483GHz -> 3 non overlapping channels
2.  5GHz, 20MHz Channels -> 5-5.5GHz -> 25 non overlapping channels
3. 6 GHz, 20MHz Channels -> 5.82-6.99GHz -> 59 non overlapping channels

> In 5,6GHz bands, there are groups of channels, each group is called a uni-band.
> channel bonding is when we have multiple channels to increase our throughput:
> 1. 20MHz -> 1xThroughput -> 1 channel
> 2. 40MHz -> 2xThroughput -> 2 channels
> 3. 80MHz -> 4xThroughput -> 4 channels
> …

> [!NOTE]
> There are other bands, but they are illegal to use in WIFI, because those frequencies are meant for other thigs such as TV, Radio, …

> The higher the frequency, the lower the distance but higher BW.

> *FSPL => Free Space Path Loss*

> [!NOTE]
> Wireless links are all working on half duplex.

> *CSMA/CA => Carrier Sense Multiple Access with Collision Avoidance* -> in wireless
> *CSMA/CD => Carrier Sense Multiple Access with Collision Detection* -> in ethernet

## RF Interference

> Doesnt happen between 2 different colliding frequencies.  but if we have multiple AP's with same frequency, we should use each AP in a different channel so they dont have interference.
> each channel is a frequency range, for example in 2.4GHz frequency -> channel 1 is 2401MHz-2423MHz

> [!NOTE]
> Channels are 5MHz apart, for example:
> 1. channel1 is 2401-2423MHz
> 2. channel2 is 2406-2428MHz
> so channels near each other still have interference, but if we want to have channels with no interference we should use each 5 channel, for example channels 1,6,11,… have no interference with each other.

## Wireless dBm Calculation

1. mWx10 -> +10dBm
2. mW/10 -> -10dBm
3. mWx2 -> +3dBm
4. mW/2 -> -3dBm

> Positive values of dBm are for Tx and Negative values are for Rx.
> 1mW is 0dBm and we can calculate dBm of signal by x/10 and x/2 of 1mW to get to our given mW and dBm.

### Likely Rx dBm

1. -50dBm -> Excelent
2. -60dBm -> Great
3. -70dBm -> Good
4. -80dBm -> Bad

> *AP => Access Point*

> [!NOTE]
> 802.3 is Ethernet standard and 802.11 is Wireless Ethernet standard.

## WIFI Standards

1. 802.11: 2.4 GHz -> 1,2 Mbps
2. 802.11a: 5 GHz -> 54 Mbps -> doesnt support 802.11
3. 802.11b: 2.4 GHz -> 1,2,5.5,11 Mbps -> supports 802.11
4. 802.11g: 2.4 GHz -> 1,2,5.5,11,54 Mbps -> supports 802.11,802.11b
5. 802.11n(WIFI 4): 2.4,5 GHz -> 600 Mbps -> bridge between all older tech, full backwards compatibility, MIMO up to 4xthroughput, Channel Bonding (20,40 MHz), HT
6. 802.11ac(WIFI 5): 5 GHz -> 7Gbps -> VHT, but also has embeded 802.11n for backwards compatibility, Channel bonding (80,160MHz), MIMO upto 8xthroughput
7. 802.11ax(WIFI6,6E): 2.4,5,6 GHz ->  -> HE, OFDMA, full backwards compatibility

> [!tip]
> WIFI 6 supports 2.4+5GHz
> WIFI 6E supports 2.4+5+6GHz

> *MIMO => Multiple Input Multiple Output* -> multiple anthennas can connect to a single device (which also has multiple anthennas) to increase throughput
> *HT => High Throughput*
> *VHT => Very High Throughput*
> *HE => High Efficiency*
> *OFDMA => Orthogonal Frequency Division Multiple Access* -> devides each channel to multiple sub carriers(oposite of channel bonding), used to increase # of channels

> Cell: is the physical space that an AP can cover(only to a bondary with acceptable signal strength(dBm) and noise level)
> in wireless network design we must have overlapping of cells but their working channels must be non-overlapping.

## Noise

> also measured as Rx dBm, and must be around -90dBm and less. (so for example -80dBm is pretty high noise)

> we must calculate SNR to see if we can accept this noise or not:
> *SNR dB = AP Rx dBm - Noise Rx dBm*
> SNR >= 10dB -> good for data
> SNR >= 25dB -> good for VoIP

> when there is unacceptable noise we must either bring AP's closer (cells get smaller) or increase signal strength of AP.

> *SNR => Singnal to Noise Ratio*

## Station Roaming

1. 802.11 Roam: when a device associated with an AP roams from this AP's cell to another AP's cell, reassociation happens and it associates with new AP with higher SNR and signal strength.
2. 802.11r: has FSR and reassociation has no delay
3. 802.11k: has assisted roaming that gives the device a list of APs and their signal strength. this helps with better reassociation.

> *FSR => Fast Secure Roaming*

> [!NOTE]
> 802.11 Roam reassociation has a 1-2 seconds delay, we might not notice it when using data, but in VoIP this delay is pretty high. 802.11r has fixed delay problem.

> [!NOTE]
> 802.11r and 802.11k can be used at same time for better performance

## SSID

> *SSID => Service Set ID*
> it is ASCII and maximum 32 chars, its a network name that can have its own security, network, BW properties.
> each AP can have many SSIDs but its recommended to use 2-3 SSIDs at most.
> also multiple APs can share same SSID.
> each SSID is mapped into a VLAN.

### Service Set

> 1. *BSS => Basic Service Set* -> single AP, doesnt have roaming
> 	1. SSID: ASCII
> 	2. BSSID: AP MAC Address
> 2. *ESS => Extended Service Set* -> multiple APs, with same SSID, has roaming, all APs are connected to a DS
> 	1. SSID: ASCII
> 	2. ESSID: same as SSID
> 	3. BSSID: per each AP
> 3. *IBSS => Independent Basic Service Set*
> 4. *MBSS => Mesh Basic Service Set*

> *DS => Distribution System* all AP's are connected to, destination of wireless traffic. it can be wireless too or wired on ethernet
> *DSM => Destribution System Medium* -> copper or wireless

## Wireless Security

1. Authentication: must be mutual(user must give usr/pass, AP must give certificate)
2. Encryption

> *WEP => Wired Equivalent Privacy* -> has low security and depricated, RC4 enc
> *WPA => WIFI Protected Access* -> more security, uses TKIP for encryption and 802.1x for authentication
> WPA2 -> uses AES for encryption and 802.1x for authentication
> WPA3 -> fixes some security problems of WPA2,AES-256 enc, SAE, OWE

> *OWE => Opportunistic Wireless Encryption* -> when we have open authentication (no PSK or SAE) but encryption still happens on traffic with DH(session key is created for enc)

> *SAE => Simultaneous Authentication of Equals* -> Both the client and AP prove knowledge of the password without transmitting the password itself and a fresh session key is generated for each connection.

> [!NOTE]
> in WIFI 5 OWE and SAE are optional but on WIFI 6/6E, OWE and SAE are mandatory and must implement. so remember this that all devices must support it.

### WPA Types

1. Personal: simple, uses PSK for auth and enc, not for large scales
2. Enterprise: complex, use 802.1x and EAP, has unique encryption key per stations

> *PSK => Pre Shared Key*
> *EAP => Extensible Authentication Protocol* -> a framework for auth mechanisms that has all kind of flavors:
> 	1. EAP-TLS
> 	2. EAP-TTLS
> 	3. PEAP
> 	4. LEAP
> 	5. EAP-Fast

![[802.1x.excalidraw|800]]

> *RADIUS => Remote Auth Dial In User Service*
> *TACACS => Terminal Access Controller Access Control System*

## Wireless Devices

1. AP: on one hand connected to STAs and on other hand connected to infrastructure with ethernet(can be PoE)
	1. Indoor: plastic, low cost, internal antenna
	2. Outdoor: metal, higher cost, external antenna
2. STA: clients that connect to AP
3. MAP: on one hand connects to STAs and on the other connects to infrastructure (to a RAP), both hands are wireless
4. RAP: if we have multiple MAPs, their infrastructure hand is towards RAP
5. Wireless bridge: connects 2 LANs
6. Anthenna:
	1. Omni-directional: 360 degree
	2. Semi-directional: one direction that anthenna is facing
		1. Planar: shorter range
		2. Yagi: longer range
	3. Dish: for satellite or wireless bridge anthennas
7. WLC: manage multiple APs in network centrally, has web ui(HTTP/HTTPS) and cli(Console/SSH)

![[Wireless Devices.excalidraw|800]]

> *STA => STAtion*
> *MAP => Mesh Access Point*
> *RAP => Root Access Point*
> *WLC => Wireless LAN Controller*
> WLC has RRM that can do self healing(if an AP goes down other APs increase their power to compensate for that lost signal) and dynamic tuning(if the more people are connecting, increase Tx power).
> *RRM => Radio Resource Management*
