---
title: TCP Dump 
category: Scanning
order: 1
---

> **TCP Dump** 

Example Command	| tcpdump -nnX tcp and port 80 and host 10.10.10.10
Protocols | ether, ip, ip6, arp, rarp, tcp, udp
Type | host, net, port, portrange
Direction | src, dst
Use numbers for machines and ports | -nn
Write to file | -w
print out in ascII or hex | -A -X
Choose interface | -i <interface>
Full packets | -s 
Network filter | Net 10.10.10

> **NMAP**

example command | ./nmap -n -A -st -p 1-1024 10.10.10.1-255
Ping | -Pn (no ping), -sP (ping sweep)
Scans | -sS (syn), -sT (TCP), -sF (FIN), -sX (FIN,PUSH, URG), -sM (FIN, ACK), -sU (UDP)
Check for firewall | --badsum <cr> (If you recieve a rest or ICMP unreachable, it is likely a firewall)
Numbers instead of machine | -n
Choose ports | -p <start>-<end>
timing | -T <0-5>
Store output | -oN (human readable), -oG (grepable), -oX (XML), -oA (all formats)
Fingerprint | -O
Version scaning | -sV
Fingerprint, version, script, and traceeroute | -A
Reason for result | --reason

> **SCAPY**

Example command | srloop(IP(dst="10.10.10.20")/ICMP()/"hello hello hello")
List Protocols | ls()
Fileds of a protocol | ls(<protocol>)
List functions | lsc
Help on function | help(<function>)
Crafta packet | packet=IP(dst="10.10.10.50")/TCP(dport=22)/"Hello"
Look at a packet | ls(packet)
Summary of packet | packet.summary()
Details on packet | packet.show()
Change a packet value | packet.sport=443, packet[TCP].flags="SA"
Multiple Targets | packet=IP(dst=[""10.10.10.1"",""10.10.10.7""]) <cr> packet=IP(dst=""10.10.10/24"")
Multiple Ports | "packet=IP(dst=""10.10.10.50"")/TCP(dport=(1,22)) <ports 1 - 22> <cr> packet=IP(dst=""10.10.10.50"")/TCP(dport=[1,22]) <ports 1 and 22>"
Send Layer 3 and higher | send()

Send Layer 2 | sendp()

Send and wait for response | sr() or srp() or sr1() <for one response>

BPF packet filter | Sr(packet, filter="host 10.10.10.50 and port 22")

Retry | Sr(Packet, retry=2)

Timeout in seconds | Sr(Packet, timeout=.1)

Interface | Sr(Packet, iface="eth0")

Response | Ans, unans=sr


