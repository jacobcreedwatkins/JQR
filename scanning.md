# JQR's
## 1.2.63 (U) Demonstrate scanning a target to identify all open ports of interest

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
1. `open`            : indicates that an application is actively accepting TCP connections, UDP datagrams
2. `closed`          : A closed port is accessible (it receives and responds to Nmap probe packets), but there is no application listening on it. the port is still reachable
3. `filtered`        : Nmap cannot determine if the port is open because packet filtering prevents its probes from reaching the port. The filtering could be from a dedicated firewall device, router rules, or host-based firewall software; provides the least amount of information
4. `unfiltered`      : indicates that a port is accessible, but Nmap is unable to determine whether it is open or closed. Only the ACK scan, which is used to map firewall rulesets, classifies ports into this state
5. `open|filtered`   : indicates that `nmap` is unable to determine whether a port is open or filtered. This occurs for scan types in which open ports give no response, including the UDP, IP protocol, FIN, NULL, and Xmas scans
6. `closed|filtered` : state is used when Nmap is unable to determine whether a port is closed or filtered. It is only used for the IP ID idle scan, using the `-sI` option


### Syntax Builder
`nmap [ <Scan Type> ...] [ <Options> ] { <target specification> }`
 - note: entire networks OR individual hosts can be scanned within the target specification
 - as a memory aid, port scan type options are of the form `-s<C>`, where `<C>` is a prominent character in the scan name, usually the first character of the scan

# Specific Port Scans
| Syntax |            Scan          |          Additional Information                                                                                                                                                                                                                                                           |
|--------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-sS`	 |	TCP SYN scan			          | Also referred to as half open scan. Most popular and default scan due to speed, relative stealth, and clear/reliable differentiation between the open, closed, and filtered states. This scan requires root priviliges since it uses raw sockets.                                         |
| `-sT`	 |	TCP connect scan		       | This is the default TCP scan type when SYN scan is not an option (i.e when super user privileges are unavailable). Takes longer and requires more packets to obtain the same information, and target machines are more likely to log the connection. Generally worse than a TCP SYN scan. |
| `-sU`	 |	UDP scans				            | Sends a UDP packet to every targetted port. UDP scanning is generally slower and more difficult than TCP, but should not be ignored as exploitable UDP services are quite common.                                                                                                          |
| `-sY`	 |	SCTP INIT scan			        |	SCTP INIT scan is the SCTP (Stream Control Transmission Protocol) equivalent of a TCP SYN scan. Can be performed quickly, scanning thousands of ports per second on a fast network not hampered by restrictive firewalls. Relatively unobtrusive and stealthy, since it never completes SCTP associations (aka "half-open"). Allows clear, reliable differentiation between the open, closed, and filtered states.
| `-sA`  | TCP ACK scan             | Used to map out firewall rulesets, determining whether they are stateful or not and which ports are filtered. When scanning unfiltered systems, open and closed ports will both return a RST packet. Nmap then labels them as unfiltered, meaning that they are reachable by the ACK packet, but whether they are open or closed is undetermined. Does NOT determine open ports, or even `open|filtered` ports!
| `-sW`  | TCP Window scan          | Works just like an ACK scan, but can also determine if a port is open. It does this by examining the TCP Window field of the RST packets returned. Instead of always listing a port as unfiltered when it receives a RST back, a Window scan lists the port as `open` or `closed`. Very specialized scan that will NOT work reliably on all target machines.
| `-sM`  | TCP Maimon scan          | Works exactly the same as NULL, FIN, and Xmas scans, except that the probe is FIN/ACK.
| `-sI`  | Idle Scan                | no packets are sent to the target from your real IP address. often used with `-Pn` for additional obfuscation syntax is: `-sI <zombie host>[:<probeport>] (idle scan)`
 
## TCP Flag Scans
- altough 3 scans are built in with manipulated TCP flag bits, any sort of custom TCP scan can be built using the `--scanflags` option.
- example syntax as follows: `--scanflags URGACKPSHRSTSYNFIN` : would set all TCP flag bits. this is pointless and just to show what can be done
- 9 (PSH and FIN)
| Syntax |       Scan               |                                                Additional Information                                                      |
|--------|--------------------------|----------------------------------------------------------------------------------------------------------------------------|
| `-sN`	 |	TCP NULL scan			         | Does not set any TCP flag bits. Cannot distinguish open ports from certain filtered ones, giving response `open|filtered`  | 
| `-sF`  | TCP FIN scan			          | Sets only the TCP FIN bit                                                                                                  |
| `-sX`	 | XMAS scan			 	           | Sets the FIN, PSH, and URG flags, lighting the packet up like a christmas tree in wireshark.                               |
 
 ### Additional Options

 - `-Pn` : pingless scan. This will skip the Nmap host discovery stage altogether. Normally, Nmap uses this stage to determine active machines for heavier scanning, but ths may be pointless if hosts are already known.
 
- `-T<1,2,3,4,5>` : Timing options, where the number between 1 and 5 determines aggressiveness of the scan.
 
- Note about SCTP: Stream Control Transmission Protocol is a third transport layer standard besides TCP and UDP. It is a transport layer protocol that is message-driven like UDP, but reliable like TCP. Typically only used by backend telecom systems, so a `-sY` scan is highly specialized.
 
 
| number |     speed   |   purpose/description                               |
|--------|-------------|-----------------------------------------------------|
|    0   | paranoid    | IDS evasion                                         |
|    1   | sneaky      | IDS evasion                                         |
|    2   | polite      | use less bandwidth and target machine resources     |
|    3   | normal      | default scan speed                                  |
|    4   | aggressive  | speeds up scan                                      |
|    5   | insane      | sacrifices accuracy for speed                       |
 
# Demo
 
 ```
> dig scanme.nmap.org                :    find the A record of the scanme.nmap.org FQDN for scanning
> nmap -sS 45.33.32.156              :    perform an Nmap SYN scan on the resolved IP address of scanme.nmap.org , this scan will NOT work and will return that you require sudo priviliges to run this scan because it uses RAW sockets.
> man raw | egrep EPERM -A1          :    this command shows the literal proof of needing root privileges to interact with raw sockets. raw sockets require root privileges because they can be used to break many basic assumptions or rules of networking, such as running a server on any port (using well known ports for different services). normal users will not need to use raw sockets for almost anything and as a security measure they therefore require root priviliges.
> sudo nmap -sS 45.33.32.156         :    this command will show the TCP SYN half open scan properly working now that sudo privileges were used.
> nmap -sT 45.33.32.156              :    demonstrates a TCP full connect scan. note that it does not require sudo privileges!
> nmap -sU 45.33.32.156              :    demonstrates a UDP scan. note the additional UDP port 123 is now found, which did NOT show in our previous scans. online online research shows us that this is the port used by the Network Time Protocol.
use the following scans: -sS, -sT, -sU. Show how the -sT scan requires super user priviliges in order to run since it uses raw sockets. 
 
 ```

- For my demonstration of port scanning I used Nmap on a Ubuntu 22.04 virtual machine to scan scanme.nmap.org for ports of interest. I demonstrated various scans you can perform using Nmap, including a TCP SYN scan, a TCP connect scan, and a UDP scan. 


# References
[nmap.org](https://nmap.org) - Nmap official website, contains all of the information you could ever possible need about Nmap

# Hours Log
|  Hours  |                Task                       |
|---------|-------------------------------------------|
| 8 hours | research, writeup creation and formatting |
| 2 hours | demonstration testing, VM configuration   |
