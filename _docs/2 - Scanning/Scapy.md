---
title: Scapy 
category: Scanning
order: 3
---

> **SCAPY**

Example command | srloop(IP(dst="10.10.10.20")/ICMP()/"hello hello hello")
List Protocols | ls()
Fileds of a protocol | ls([protocol])
List functions | lsc
Help on function | help([function])
Crafta packet | packet=IP(dst="10.10.10.50")/TCP(dport=22)/"Hello"
Look at a packet | ls(packet)
Summary of packet | packet.summary()
Details on packet | packet.show()
Change a packet value | packet.sport=443, packet[TCP].flags="SA"
Multiple Targets | packet=IP(dst=[""10.10.10.1"",""10.10.10.7""]) <br> packet=IP(dst=""10.10.10/24"")
Multiple Ports | <code>//ports 1 - 22 <br> packet=IP(dst=""10.10.10.50"")/TCP(dport=(1,22))  <br>//ports 1 and 22 <br>packet=IP(dst=""10.10.10.50"")/TCP(dport=[1,22]) " </code>
Send Layer 3 and higher | send()
Send Layer 2 | sendp()
Send and wait for response | sr() or srp() or sr1() <for one response>
BPF packet filter | sr(packet, filter="host 10.10.10.50 and port 22")
Retry | sr(Packet, retry=2)
Timeout in seconds | sr(Packet, timeout=.1)
Interface | sr(Packet, iface="eth0")
Response | sns, unans=sr

