# Automation Intro

> Push: central control station sends commands to devices without any agent. (eg. Ansible)
> Pull: devices send some data to central control station using agent. (eg. Puppet, Chef)

> Ansible works based on Python but Puppet and Chef are based on Ruby. these tools are used to automate configuring OS of any VM or Device.
> Terraform is used to automate configuring infrastructure, creating resources.

## AI

1. Generative: configure devices or write programs
2. Predictive: predictions based on large amount of data, to detect anomalies and traffic spikes.

> REST API is a stateless server client model that lets us use API in form of HTTP/HTTPS.

## Structured Data

1. JSON
2. YAML
3. XML

> RESTCONF is a cisco service that lets us get structured info form cisco devices.

## SDN

> *SDN => Software Defined Network* -> Centralizes all the control planes of devices in network but data plain of each device is on them.

### Logical Planes

1. Data Plane: movin and forwarding packets and frames -> fast, done by ACIS chip on devices,  moving muscles
2. Control Plane: processing routing and arp desicions and doing any other desicions about different protocols -> thinking brain
3. Management Plane: doing the work for console management and remote management, syslog, ntp

> central control plain in SDN uses API to control the network nodes.

### SDN Logical Interfaces

1. Southbound: cummunication of controller to nework nodes (pushing config, …)
2. Northbound: controller interactions with external applications outside our network

## SD Access

> Cisco SDN solution for campus networks

### Components

1. underlay: IGP routing protocols
2. overlay: virtual networks like VPN, VXLAN
3. fabric: combination of under/overlay that is managed by a controller -> Cisco DNA Center
	1. fabric edge node: the node where end user systems are connected
	2. fabric border node: the node where our fabric connects to another network
	3. fabric control node: the node that provides some control plane funcionality (eg. running LISP)
