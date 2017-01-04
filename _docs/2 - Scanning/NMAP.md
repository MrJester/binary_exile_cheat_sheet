---
title: NMAP
category: Scanning
order: 2
---

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

> **NSE**

Example Command | ./NMAP -n -sV --script=[all, category, dir, script...] [target] -p [ports]
All scripts  | ./NMAP -sC [target] -p [ports]
Script details | --script-trace
Script help | --script-help
Script arguments | --script-args[arguments]
Script database | /opt/NMAP/share/NMAP/scripts/script.db
Script files | /opt/NMAP/share/NMAP/scripts/<name>.nse




