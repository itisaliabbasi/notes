# SNMP

> *SNMP => Simple Network Management Protocol*
> *NMS => Network Management Station* -> SNMP Manager
> *MIB => MGMT Information Base* -> a database of OID's that each device (SNMP Agent) has and sends those info  to NMS
> *OID => Object ID* -> ID of an specific data about the device
> SNMP Trap is when an alarm is triggered and some info about that particular event is sent to NMS.
> SNMP Inform is when an alarm is triggered and some info about that particular event is sent to NMS.

> [!NOTE]
> SNMP Inform will be sent repeatedly until NMS gives an ACK to SNMP Agent but SNMP Trap wont be sent again, so SNMP Inform is more reliable and SNMP Trap uses less resource.

## Versions

1. v2c: uses cummunity string in plain text, insecure
2. v3: uses hashed auth and encryption, secure
	1. view: scope of access (OID's)
	2. group: list of users with read/write permissions on a view with security type(no auth no priv, auth no priv, auth priv)
	3. users: user+pass, encryption key, hash type

## SNMP V2 Config

```
conf t
snmp-server contact <contact info for this node>
snmp-server location <location or geo address of this node>
snmp-server chassis-id <asset tag of this device>
show snmp contact
show snmp location
show snmp chassis
snmp-server community <cummunity_string> <ACL_name> <permission>
snmp-server host <NMS_IP> version 2c <cummunity_string>
snmp-server host <NMS_IP> informs version 2c <cummunity_string>
snmp-server host <NMS_IP> traps version 2c <cummunity_string>
snmp-server enable traps //enables both traps and informs (based on previous lines)
```

## SNMP V3 Config

```
conf t
snmp-server contact <contact info for this node>
snmp-server location <location or geo address of this node>
snmp-server chassis-id <asset tag of this device>
show snmp contact
show snmp location
show snmp chassis
snmp-server view <view_name> iso included
snmp-server group <group_name> v3 <security_type> <permission> <view_name>
snmp-server user <user_name> <group_name> v3 auth <hash_algo> <hash_password> access <acl_name> priv <enc_algo> <key_size> <enc_password>
```
