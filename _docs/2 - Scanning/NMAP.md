---
title: Port Scanning
category: Scanning
order: 2
---

> **NMAP**

example command |  
```cpp
nohup ./nmap -n --randomize-hosts --reasons -A -st -vv -oA [OUTPUTFILE] -p 1-1024 10.10.10.1-255 > output.txt &
 ```
example command 2 | <code> nohup ./nmap -n --randomize-hosts --reasons -A -st -vv -oA [OUTPUTFILE] > -p 1-1024 -iL [HOSTLIST]  > output.txt & </code>
Ping |  -Pn (no ping), -sP (ping sweep) 
Scans | -sS (syn), -sT (TCP), -sF (FIN), -sX (FIN,PUSH, URG), -sM (FIN, ACK), -sU (UDP) 
Check for firewall |  --badsum <br> (If you recieve a rest or ICMP unreachable, it is likely a firewall) 
Numbers instead of machine |  -n 
Choose ports | -p [START]-[END]
timing | -T (0-5)
Store output | -oN (human readable), -oG (grepable), -oX (XML), -oA (all formats)
Fingerprint |  -O
Version scaning | -sV 
Fingerprint, version, script, and traceeroute | -A 
Reason for result | --reason 

> **NSE**

Example Command | <code> ./NMAP -n -sV --script=[all, category, dir, script...] [target] -p [ports] </code>
All scripts  | <code> ./NMAP -sC [target] -p [ports] </code>
Script details | --script-trace 
Script help |  --script-help
Script arguments | --script-args[arguments]
Script database | /opt/NMAP/share/NMAP/scripts/script.db 
Script files | </opt/NMAP/share/NMAP/scripts/<name>.nse

> **MASSCAN**

Example Command | <code> nohup masscan -p [PORTS] --rate 500 -vv --includefile [HOSTLIST] --output-format list --output-filename [OUTPUTFILE] > output.txt & </code>


> **Usefule Resources**

* scanrand
* zmap

