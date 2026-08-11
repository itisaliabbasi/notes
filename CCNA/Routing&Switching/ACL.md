# ACL

> *ACL => Access Control List*
> 1. Standard ACL: only has src ip and info on ACL.
> 2. Extended: has src/dst ip/port and protocols. (any thing in L3&L4)

## Standard ACL Config

```
conf t
ip access-list standard <ACL-Name>
	<permit|deny> <ip>
	<permit|deny> <net-id> wildcard bits 0.0.0.255
	deny any log //implicit deny
	exit
int gig 0/0
	ip access-group <ACL-name> <in|out>
	exit
line vty 0 4
	access-class <ACL-name> in //only accept management station ip for remote sessions
	exit
sh access-lists 
ip access-list resequence <ACL-name> <start-seq> <seq-steps> //changes the sequence number of ACL records in case we want to change the order of our ACLs
```

## Extended ACL Config

```
ip access-list extended <ACL-name>
	<permit|deny> <protocol> <src-ip> <src-wildcard-bits> <dst-ip> <dst-wildcard-bits> <eq|neq|...> <port-number>
	exit
int gig 0/0
	ip access-group <ACL-name> <in|out>
	exit
sh access-lists
```
