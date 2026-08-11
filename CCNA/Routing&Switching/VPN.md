# VPN

> *VPN => Virtual Private Netwrok*
> 1. Site to Site
> 2. Remote Access

## VPN Protocols

> 1. *GRE => Generic Routin Encapsulation*
> 2. IPSec
> 3. SSL/TLS
> 4. DMVPN

### IPSec

1. Transport: only encrypt payload, not header
2. Tunnel: encrypt whole packet and adds its own header

> [!NOTE]
> Its Recommended to use Loopback interfaces for Tunnels, because if we have multiple paths to destination, we dont need to config multiple tunnels and if one interface goes down Loopback is still reachable through other interfaces. so the load balance or failover is done by underlay routing, not the tunnels.
