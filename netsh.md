# JQR's
##### 1.2.34 (U) : Given an operational environment, demonstrate the use of redirection by netsh to forward traffic to a target device
##### 1.2.35 (U) : Explain the limitations on the use of netsh for redirection of network traffic 

#### Things I will need:
- windows 2kXX server vm
- windows 10 vm (host)
- netsh itself (obviously)


# netsh preliminary research - what actually is it?
- `netsh` or network shell is a windows command line utility allowing for local and/or remote configuaration of network devices and interfaces
- one example of something you can do with `netsh` is change the IP address on your machine or edit wireless settings like changing your SSID
- first included in Windows 2000 and now a staple functionality part of every major Windows NT (new technology) release afterward
- Layman's terms: allows you to display or modify the network configuration of a computer that is currently running


## In the context of forwarding:
- netsh with portproxy can be used similar to ssh or iptables 

## example
- Suppose you had a web server running locally on port 80, and it only binds on 127.0.0.1 (loopback). If you wanted to tunnel that traffic out on a remote interface:
- `netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=48333 connectaddress=127.0.0.1 connectport=80`
  	
- the above command would: invoke the netsh command and add a portproxy interface, going from an ipv4 to another ipv4 address, with a listening IP address of 0.0.0.0,
a listening port of 48333, and a connect address of 127.0.0.1 on port 80.

##### base syntax
`netsh interface portproxy add v4tov4 listenaddress=localaddress listenport=localport connectaddress=destaddress connectport=destport`

# References
- [Microsoft netsh Guide](https://docs.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh-contexts)
- [Embrace The Red's Port Forwarding and Port-Proxying on Windows](https://embracethered.com/blog/posts/2020/windows-port-forward/)
- [Windows OS Hub Port Forwarding Guide](http://woshub.com/port-forwarding-in-windows/) 


## Hours Log
| Hours | Task |
|-------|------|
| 3 hours| Research, writeup creation|
| 6 hours| configuring VM's, setting up demo, creating demo|


## VMWare Stuff

`netsh firewall set icmpsetting 8 enable`

^ this command helped my two host windows VM's talk to each other, it seemed that the firewall was getting in the way of the network communicating


