---
title: Port Scanning
category: Scanning
order: 2
---

> **NMAP**

example command | <code> nohup ./nmap -n --randomize-hosts --reasons -A -st -vv -oA (OUTPUTFILE) -p 1-1024 10.10.10.1-255 > output.txt &</code>
example command 2 | <code> nohup ./nmap -n --randomize-hosts --reasons -A -st -vv -oA (OUTPUTFILE) > -p 1-1024 -iL (HOSTLIST)  > output.txt & </code>
Ping | <code> -Pn (no ping), -sP (ping sweep) </code>
Scans | <code> -sS (syn), -sT (TCP), -sF (FIN), -sX (FIN,PUSH, URG), -sM (FIN, ACK), -sU (UDP) </code>
Check for firewall | <code> --badsum <br> (If you recieve a rest or ICMP unreachable, it is likely a firewall) </code>
Numbers instead of machine | <code> -n </code>
Choose ports | <code> -p (START)-(END) </code>
timing | <code> -T (0-5) </code>
Store output | <code> -oN (human readable), -oG (grepable), -oX (XML), -oA (all formats)</code>
Fingerprint | <code>  -O
Version scaning | <code> -sV </code>
Fingerprint, version, script, and traceeroute | <code> -A </code>
Reason for result | <code> --reason </code>

> **NSE**

Example Command | <code> ./NMAP -n -sV --script=[all, category, dir, script...] [target] -p [ports] </code>
All scripts  | <code> ./NMAP -sC [target] -p [ports] </code>
Script details | <code> --script-trace </code>
Script help | <code> --script-help </code>
Script arguments | <code> --script-args[arguments] </code>
Script database | <code> /opt/NMAP/share/NMAP/scripts/script.db </code>
Script files | <code> /opt/NMAP/share/NMAP/scripts/<name>.nse </code>

> **MASSCAN**

Example Command | <code> nohup masscan -p (PORTS) --rate 500 -vv --includefile (HOSTLIST) --output-format list --output-filename (OUTPUTFILE) > output.txt & </code>


> **Usefule Resources**

* scanrand
* zmap

