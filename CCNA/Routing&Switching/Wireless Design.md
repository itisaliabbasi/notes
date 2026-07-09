# Wireless Design

1. Autonomous: each AP is configured and managed individually and works on its own
2. Lightweight: use WLC to manage and configure APs centrally.

> *CAPWAP => Control And Provisioning of Wireless Access Points* -> modern, widely used
> *LWAPP => Light Weight Access Point Protocol* -> old, deppricated

## CAPWAP

![[CAPWAP.excalidraw|800]]

> has 2 tunnels, one for config data(WLC sends config, AP sends data on rogue APs, SNR, interference, …) and other one for STA data(in this tunnel, frames from STA is directly transfered to WLC and then goes in network, and routed or filtered)
> *PoP => Point of Presence* -> where STA packets are seen in network, in Lightweight design its where WLC is in network -> so the STA VLAN is the VLAN that WLC is in

### CAPWAP Tunnels Benefits

1. Isolated management traffic
2. Eliminate L3 roaming: because STA traffic is sent to WLC in L2 and then routed, so roaming  doesnt affect it much
3. Centralized VLAN&Subnets: all APs can be on same VLAN through our organization

## AP Modes of Operation

1. local: used in lightweight architecture, AP connects to WLC, CAPWAP Control and Data tunnels
2. flexconnect: CAPWAP only contro tunnel, in case of WLC failure flexconnect stays online, it has limitations so not recommended for everywhere, better for small remote sites
3. monitor: sends data on RF environment to WLC(rogue AP, SNR, noise, interference), doesnt give service to STAs, its high cost
4. sniffer: helps with tshoot and captures packets
5. bridge: used when 2 sites want to connect using APs, or can be used in MAPs
6. flex+bridge: used in MAPs to deliver flexconnect to mesh AP operations

> [!NOTE]
> In a single site we can either have felxconnect ot local mode for all APs. we cant implement them together.

## AP Links

1. autonomous architecture: swtich port to AP must be trunk and allowed VLANs must be the ones associated with SSIDs
2. lightweight architecture:
	1. local mode: switchport to AP must be access to a VLAN, that VLAN must be able to see WLC and WLC link to switch must be trunk to pass VLANs associated with SSIDs
	2. flexconnect: like autonomous architecture
	3. monitor/sniffer/bridge modes: switchport to AP must be access to a VLAN that can see WLC

> [!NOTE]
> Older WLCs use aireOS but new ones use IOS-XE. also nowadays vWLC is used. also EWC can be used to make an AP a WLC in network.

> *EWC => Embeded Wireless Controller*

## WLC Discovery

1. broadcast: AP and WLC are on same VLAN & Subnet so they can see each other with broadcast
2. DHCP: AP can take data on WLC through DHCP option 43(WLC IP addresses)
3. DNS query: CISCO-CAPWAP-CONTROLLER.domain.local record must be in DNS server and points to WLC ip address, so AP can query it from DNS server.
4. manual: configure WLC ip address manually in AP

> goal of discovery is to create a list of up to 3 WLC IPs, so AP can have other WLC in case of failure.

> [!NOTE]
> Its recommended to have multiple WLCs (physical or virtual) to avoid SPF.
> and also use aggregation on links between WLC and infrastructure.

> [!NOTE]
> AireOS only suppurts ehterchannel mode ON(no LACP and PAGP).

## WLC Ports for AireOS

1. physical ports:
	1. DP: inband ports, VLANs, SSIDs, to LAN, Trunk, better be in LAG, CAPWAP tunnels are though this ports
	2. SP: used for out of band management for WLC, access to MGMT VLAN
	3. RP: HA ports for WLC(active/passive only), SSO
2. logical interfaces:
	1. management: binded to a DP, as long as DP is online management is also online, used by APs for CAPWAP tunnels
	2. virtual interface: unreachable IP, used for internal functionality
	3. service port: has an ip and binded to SP(SP it self cant have ip, so this ip binds to it)
	4. redundancy port: has an ip and binded to RP
	5. radundancy management port: binded to DPs so they send each other keepalive packets on DPs, to make sure AP traffic can reach actice WLC
	6. dynamic interfce: created by admin and mapped to a VLAN and binded to a DP

> *DP => Distribution Port*
> *SP => Service Port*
> *RP => Redundancy Port*
> *SSO => Statefull Switch Over*
