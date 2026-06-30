# CDP & LLDP

> *CDP => Cisco Discovery Protocol*
> *LLDP => Link Layer Discovery Protocol*

> `show cdp` shows global CDP info, `show cdp neighbors` shows all CDP devices connected and the interface we see them on, `show cdp neighbors detail` shows more detail on them.

> `no cdp run` globaly disables CDP, `cdp run` enables it. `no cdp enable` on an interface only disables CDP on that interface.

> `show lldp`, `lldp run`, `show lldp neighbors`, `show lldp neighbors detail`, `no lldp run` are LLDP related commands.

> `show power inline` on PoE devices shows how much power on each interface is used.

> `no lldp [transmit|recieve]` on an interface, disables it specifically on an interface.
