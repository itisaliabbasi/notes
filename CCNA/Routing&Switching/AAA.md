# AAA

> *AAA => Authentication, Authorization, Accounting*

## AAA Protocols

1. 802.1x: AAA for network access(auth on link connection)
2. RADIUS: open standard, works on L4 and UDP, only password is encrypted between device and server, all other info are plain text. usually used for end users in remote access and enterprise wireless neworks. lumps AAA together.
3. TACACS+: developed by cisco, works on L4 and TCP, whole payload is encrypted. does AAA separately. usually used for management networks and managing devices.
