# JQR's
##### 1.2.63 (U) Demonstrate scanning a target to identify all open ports of interest

# Port Scanning with `nmap` (network mapper)

- to demonstrate port scanning, I used `nmap`
- `nmap` or network mapper is an open source tool for network exploration and security auditing
- it is designed to rapidly scan large networks or individual hosts


### `nmap` output

- example output after running a scan will look something like the block below:

```
jacob@jacob-virtual-machine:~$ nmap -T4 192.168.210.129
Starting Nmap 7.80 ( https://nmap.org ) at 2022-07-18 10:43 EDT
Nmap scan report for 192.168.210.129
Host is up (0.0011s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```

|        PORT       |                      STATE                           |                           SERVICE                            |
|-------------------|------------------------------------------------------|--------------------------------------------------------------|
| port / udp or tcp | open, closed, filtered, unfiltered, open\|filtered, and closed\|filtered; more info below  | nmap's best approximation of the service running on the port |

- note that since services can run on alternate ports than their correspondiong well-known port, the services category should be taken with a grain of salt.
- 

### Syntax Builder
`nmap [ <Scan Type> ...] [ <Options> ] { <target specification> }`
 - note: entire networks OR individual hosts can be scanned within the target specification

# Demo

- For my demonstration of portscanning I set up a Ubuntu 22.04 machine scanning a Windows 10 Home machine for ports of interest. I demonstrated various scans you can perform using nmap, including a XMAS tree scan, a TCP half-open scan, and various speeds of scanning. Additonally, I scanned the entire network to show `nmap`'s functionality for scanning entire networks over individual boxes.


# References
[nmap.org](https://nmap.org)

# Hours Log
| Hours | Task |
|-------|------|
| x hours | x task |
