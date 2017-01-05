---
title: Port Scanning
category: Scanning
order: 2
---

> **NMAP**

example command | nohup ./nmap -n --randomize-hosts --reasons -A -st -vv -oA <output file> -p 1-1024 10.10.10.1-255 > output.txt &&
example command 2 | nohup ./nmap -n --randomize-hosts --reasons -A -st -vv -oA <output file> -p 1-1024 -iL <hostlist>  > output.txt &
Ping | -Pn (no ping), -sP (ping sweep)
Scans | -sS (syn), -sT (TCP), -sF (FIN), -sX (FIN,PUSH, URG), -sM (FIN, ACK), -sU (UDP)
Check for firewall | --badsum <br> (If you recieve a rest or ICMP unreachable, it is likely a firewall)
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

> **MASSCAN**

Example Command | <code> nohup masscan -p<ports> --rate 500 -vv --includefile <host list> --output-format list --output-filename <outputfile> > output.txt & <code>




