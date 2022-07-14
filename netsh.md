# JQR's
##### 1.2.34 (U) : Given an operational environment, demonstrate the use of redirection by netsh to forward traffic to a target device
##### 1.2.35 (U) : Explain the limitations on the use of netsh for redirection of network traffic 


# `netsh` preliminary research - what actually is it?
- `netsh` or network shell is a windows command line utility allowing for local and/or remote configuaration of network devices and interfaces
- one example of something you can do with `netsh` is change the IP address on your machine or edit wireless settings like changing your SSID
- first included in Windows 2000 and now a staple functionality part of every major Windows NT (new technology) release afterward
- Layman's terms: allows you to display or modify the network configuration of a computer that is currently running


## In the context of forwarding:
 The `netsh interface portproxy` command provides a command-line tool for use in administering servers that act as proxies between IPv4 and IPv6 networks and applications.
- `netsh interface portproxy` is extremely similar to ssh tunneling or manipulating iptables chains. 
- `netsh` has port forwarding capabilities with `interface portproxy`, which is our focus in the JQR, it is a very robust command line scripting utility that is primarily used for network comfiguration


### Tunneling vs. port forwarding?
Port forwarding is also often referred to as tunneling, and the two terms are used interchangeably. The encrypted "tunnel" serves as a means transfer data and deliver it safely to the remote system. This method is regularly used to circumvent standard firewall security protocols. Either terms are examples of network traffic redirection.

# Limitations of `netsh`

- `netsh` is a very powerful and useful tool; however, it comes with a few key limitations.
- most notably is that it is a proprietary Windows utility, which can only communicate with, configure, and be used with other machines using Windows operating systems. 
- potentially better alternatives to `netsh` specifically for portforwarding and redirection exist, which may be more scalable, functional, or practical for a given operation.
- despite `netsh` being very functional within Windows environments, `ssh` or secure shell also can be used in these environments, which also has cross platform compatibility with linux/unix, macOS, etc.

![image](https://user-images.githubusercontent.com/100236631/179068047-2bd65755-316e-4896-9b26-93c5b70f6cee.png)

- Windows has indicated that `netsh` functionality for TCP/IP may be deprecated in future versions of Windows. 
- Note that this message is earliest dated back to 2012 where many `netsh.exe` commands displayed the message above upon execution in Windows Server 2012 and Windows Server 2012 R2. Despite this, `netsh` has continued to be supported in every major Windows NT release since, so take this with a grain of salt.

#### base syntax of `netsh interface portproxy` for portforwarding
- `netsh interface portproxy add v4tov4 listenaddress=localaddress listenport=localport connectaddress=destaddress connectport=destport`
- `netsh interface portproxy show all` -> shows all portforwarding configurations, example output below

![image](https://user-images.githubusercontent.com/100236631/179072661-80e23c16-fb84-4326-8e7f-3fde7c6e66bc.png)
 
# DEMO
- created snapshots ready-to-go for both the Windows 10 Home (client) and Windows 10 Pro (server) virtual machines
- demonstrate the port forwarding aspect of it, explain the syntax and what is actually happening


## Demo pt. 1: Setting Up the Windows 10 Pro Server

- open an administrator powershell
- `cd ~/Environments` -> tab complete will bring you to "Environments" directory
- `.\my_env\Scripts\activate` -> this will begin the `my_env` python3 virtual environment
- `.\webserver.py` -> will actually start up the webserver, hosted on 192.168.210.129:8000


## Demo pt. 2: Using netsh port forwarding to connect to the python web server
- open an administrator powershell session
- `netsh interface portproxy add v4tov4 listenaddress=127.0.0.1 listenport=44444 connectaddress=192.168.210.129 connectport=8000` -> explain what the command is actually doing, what the syntax means in layman's terms
- open firefox and type `http://127.0.0.1:44444` into search address bar, should bring you to the webserver
- successful connection to the server indicates netsh portforwarding and demonstration of redirection

# References
- [Microsoft netsh Guide](https://docs.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh-contexts)
- [Embrace The Red's Port Forwarding and Port-Proxying on Windows](https://embracethered.com/blog/posts/2020/windows-port-forward/)
- [Windows OS Hub Port Forwarding Guide](http://woshub.com/port-forwarding-in-windows/) 
- [Set Up a local python3 programming environment in Windows 10](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-windows-10)
- [pythonbasics.org Web Server setup](https://pythonbasics.org/webserver/)
- [netsh potentially being deprecated](https://www.dell.com/support/kbdoc/en-us/000114303/recommended-use-of-windows-powershell-instead-of-network-shell-in-windows-server-2012-r2#:~:text=Many%20of%20the%20netsh.exe,and%20Windows%20Server%202012%20R2.)
- [discussion of interfaces having their own IP addresses rather than individual devices](https://networkengineering.stackexchange.com/questions/56156/why-are-ip-addresses-given-to-each-interface-and-not-device-what-would-the-impl#:~:text=And%20the%20way%20an%20interface,which%20that%20Router%20belongs%20within.)

### Hours Log
| Hours | Task |
|-------|------|
| 3 hours| initial research, writeup creation|
| 6 hours| configuring VM's, setting up demo, creating demo|
| 3.5 hours| additional configuration of virtual environment, testing demo, creating VMware snapshots, additions and revisions to writeup


