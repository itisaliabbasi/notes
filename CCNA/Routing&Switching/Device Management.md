# Device Management

> *VTY => Virtual TeletYpe*

> `show line` shows status on VTY lines
> `show users` shows all the current VTY sessions

## Line Security

1. use secret instead of password for users
2. login methods:
	1. login password
	2. login local: global user/pass
	3. no login
	4. AAA
3. ACL
4. use SSH instead of Telnet

## Line & SSH Config

```
//ssh config:
conf t
ip domain-name <domain>
ip ssh version 2
crypto key generate rsa modulus <key-size>
show ip ssh

//less secure:
conf t
line vty 0 4
	password <pass>
	login
	transport input ssh
	exit
enable secret <pass>

//more secure:
conf t
username <user> privilege 15 secret <pass>
ip access-list standart <ACL-name>
	permit <IP>
	exit
line vty 0 4
	access-class <ACL-name> in
	login local
	transport input ssh
	exit
```

> [!NOTE]
> Passwords are unencrypted in cisco and secrets are encrypted. their encryption has levels from 0(plain text) to 9(strongest encryption).
> we can always use `service password encryption` to automatically encrypt all passwords and secrets in that device.
