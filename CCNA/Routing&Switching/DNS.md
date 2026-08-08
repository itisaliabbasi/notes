# DNS

> *DNS => Domain Name Service*

## DNS Client Config

```
conf t
ip name-server <main_DNS_IP> <alternate_DNS_IP>
ip domain-name <domain_name>
ip host <name> <IP> //local DNS records (not recommended to use)
ip domain-lookup
```
