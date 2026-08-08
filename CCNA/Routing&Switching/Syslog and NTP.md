# Syslog

## Log Locations

1. Buffer: `show logging` command to see list of logs
2. Console: logs we see when we are connected using console cable
3. Monitor: `terminal monitor` in a remote session

## Log Format

1. Time Stamp: when log is generated
2. Fcility: %
3. Severity Level:
	0. emergency
	1. alert
	2. critical
	3. error
	4. warning
	5. notification
	6. information
	7. debug
4. Mnemonic: code/category of log
5. Description: human readable

> [!NOTE]
> Timestamp can be disabled with `no service timestamps` and we can use `service sequence-numbers` instead of it.

> [!NOTE]
> If we set a value for log severity, all logs with severity of <= that value will be saved.

## Logging Config

```
conf t
logging buffered <severity_level>
logging buffered <buffer_size>
logging host <syslog_server_ip>
logging <console|monitor|buffer> <severity> //to only show this severity or lower levels in specified log location
logging trap <severity_level> //to change severity of loggs sent to syslog server
line con 0
	logging synchronous //rewrite command after showing a log
	exit
line vty 0 15
	logging synchronous
	exit
```

# NTP

> *NTP => Network Time Protocol*

## NTP Security

1. ACL
2. Auth+Enc

> usually there is an authorative master NTP server in a network that all other devices and clients get their time from it. but also another device can take time from that authorative master NTP and become server to some other clients. Stratom means how many levels away is an NTP client from authorative NTP master.

> [!NOTE]
> Stratom is a number 1 to 15, and the lower it is, the more accurate the time is.

## NTP Config

```
conf t
clock timezone <timezone_name> <UTC_offset>
clock set <time> <month> <day> <year> //set time manually
ntp master <stratom_number> //configure device as master NTP server (recommended stratom is 3)
ntp server <IP> //configure device as NTP client
show ntp associations //to see if the time is synced with NTP server or not
show ntp status
show clock
```
