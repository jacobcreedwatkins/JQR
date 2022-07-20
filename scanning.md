# JQR's
##### 1.2.63 (U) Demonstrate scanning a target to identify all open ports of interest

# Port Scanning with `nmap` (network mapper)

- to demonstrate port scanning, I used `nmap`
- `nmap` or network mapper is an open source tool for network exploration and security auditing
- it is designed to rapidly scan large networks or individual hosts. although Nmap is a very robust tool with several network reconnaissance abilities beyond just port scanning, for the sake of the JQR we will be maintiaining our focus on port scanning only.


### `nmap` output

- example output after running an Nmap port scan will look something like the block below:

```
jacob@jacob-virtual-machine:~$ nmap 192.168.210.129
Starting Nmap 7.80 ( https://nmap.org ) at 2022-07-18 10:43 EDT
Nmap scan report for 192.168.210.129
Host is up (0.0011s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```




|        PORT       |                                        STATE                                               |                           SERVICE                            |
|-------------------|--------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| port / udp or tcp | open, closed, filtered, unfiltered, open\|filtered, and closed\|filtered; more info below  | nmap's best approximation of the service running on the port |

- note that since services can run on alternate ports than their correspondiong well-known port, the services category should be taken with a grain of salt.


# Port States
1. open - indicates that an application is actively accepting TCP connections, UDP datagrams
2. closed - A closed port is accessible (it receives and responds to Nmap probe packets), but there is no application listening on it. the port is still reachable
3. filtered - Nmap cannot determine if the port is open because packet filtering prevents its probes from reaching the port. The filtering could be from a dedicated firewall device, router rules, or host-based firewall software; provides the least amount of information
4. unfiltered - indicates that a port is accessible, but Nmap is unable to determine whether it is open or closed. Only the ACK scan, which is used to map firewall rulesets, classifies ports into this state
5. open\|filtered - indicates that `nmap` is unable to determine whether a port is open or filtered. This occurs for scan types in which open ports give no response, including the UDP, IP protocol, FIN, NULL, and Xmas scans
6. closed\|filtered - state is used when Nmap is unable to determine whether a port is closed or filtered. It is only used for the IP ID idle scan, using the `-sI` option


### Syntax Builder
`nmap [ <Scan Type> ...] [ <Options> ] { <target specification> }`
 - note: entire networks OR individual hosts can be scanned within the target specification
 - as a memory aid, port scan type options are of the form -s<C>, where <C> is a prominent character in the scan name, usually the first character

# Specific Port Scans
| Syntax |            Scan          |          Additional Information                                                                                                                                                                                                                                                           |
|--------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-sS`	 |	TCP SYN scan			          | Also referred to as half open scan. Most popular and default scan due to speed, relative stealth, and clear/reliable differentiation between the open, closed, and filtered states. This scan requires root priviliges since it uses raw sockets.                                         |
| `-sT`	 |	TCP connect scan		       | This is the default TCP scan type when SYN scan is not an option (i.e when super user privileges are unavailable). Takes longer and requires more packets to obtain the same information, and target machines are more likely to log the connection. Generally worse than a TCP SYN scan. |
| `-sU`	 |	UDP scans				            | Sends a UDP packet to every targetted port. UDP scanning is generally slower and more difficult than TCP, but should not be ignored as exploitable UDP services are quite common                                                                                                          |
| `-sY`	 |	SCTP INIT scan			        |	
| `-sN`	 |	TCP NULL scan			         |
| `-sF`  | TCP FIN scan			          |
| `-sX`	 | XMAS scan			 	           |
| `-sA`  | TCP ACK scan             |
| `-sW`  | TCP Window scan          |
| `-sM`  | TCP Maimon scan          |
| `-sZ`  | SCTP COOKIE ECHO scan    |
| `-sO`  | IP protocol scan         |
| `-sI`  | Idle Scan                | 
 
 
 
# Demo
 
 ```
 > dig scanme.nmap.org
 
 ```

- For my demonstration of portscanning I set up a Ubuntu 22.04 machine scanning a Windows 10 Home machine for ports of interest. I demonstrated various scans you can perform using nmap, including a XMAS tree scan, a TCP half-open scan, and various speeds of scanning. Additonally, I scanned the entire network to show `nmap`'s functionality for scanning entire networks over individual boxes.


# References
[nmap.org](https://nmap.org)

# Hours Log
| Hours | Task |
|-------|------|
| x hours | x task |
