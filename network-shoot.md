
## telnet 
 telnet connect destination’s host and port via a telnet protocol if a connection establishes means connectivity between two hosts is working fine  
 ```
  [root@lab ~]# telnet gf.dev 443
  Trying 104.27.153.44...
  Connected to gf.dev.
  ```

## tracert <ip>  [ Bottlenecks in router / router support for traceroute ]
 Use when we have some series on internal routers , also enabled to respond to tracert. And then troubleshoot path of packet to external internet etc.  
   Also which router does not have further route and breaks the packet journey.    
 
 1. List all routers on way to target IP.
 2. Few routers have hops blocked and dont reflect back their IP and times out.
 3. Observer
 
 
 ## nslookup  [ Lookup IP against  domain name & vice-versa  ]
 
 E.g home router have the DNS as the first router IP itself.
 
 > nslookup pluralsight.com  
 
 ## arp table on machine  [ admin priviledge ] 
  Arp entries for what mac address for IPs. A success ping to an IP  
 
  > arp -a  
  > arp -d *  // clear arp table and rediscover

## dig [ Linux - show dns entries ]  

## route [ Show default gateway information  ] 
> route -n  // no ip resolution

## iptables [ ] 
 Check if any rules are causing packet drops etc. Local debug turn off iptables and check.  
 
## netstat / ss [ moving to ss ]
 You can see what services are running with the netstat command. While netstat is still available, most Linux distributions are transitioning to ss command.
use ss command with -t and -a flags to list all TCP sockets. This displays both listening and non-listening sockets.
To display only TCP connections with state established:    
  > ss -a -t -o state established  
 
## ip [ moving from ifconfig ]

| commands|
|---|
|ip link|
|ip route|
|ip address|

## tcpdump [ ]

 > which tcpdump

 to see which interfaces are available for capture:  
  > tcpdump --list-interfaces (or -D for short) 
  > sudo tcpdump -D

 Capture dump   
 > tcpdump --interface any

 limit the number of packets captured and stop tcpdump, use the -c (for count) option:     
 > tcpdump -i any -c 25

 DISABLE auto resolution of IP (-n) /port (-nn)    
  > tcpdump -i any -c5 -nn

 PROTOCOL filter ( e.g icmp )  
 > tcpdump -i any -c5 icmp

 PORT filter  
 > tcpdump -i any -c5 -nn port 80

 SRC IP/Hostname filter  
 > tcpdump -i any -c5 -nn src 192.168.122.98

 Write to file  
 > tcpdump -i any -c10 -nn -w webserver.pcap port 80
 > tcpdump -i any -c10 -nn -w ssh.pcap port 22


 Read from pcap file  
 > tcpdump -nn -r webserver.pcap

---

## Trouble shoot 
  * IP Config Issues
  * DHCP issues
  * Service Issues
  * Security issues
  
  1. While assigning an IP  
