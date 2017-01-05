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

Example Command | <code> ./NMAP -n -sV --script=[all, category, dir, script...] [target] -p [ports] </code>
All scripts  | <code> ./NMAP -sC [target] -p [ports] </code>
Script details | <code> --script-trace </code>
Script help | <code> --script-help </code>
Script arguments | <code> --script-args[arguments] </code>
Script database | <code> /opt/NMAP/share/NMAP/scripts/script.db </code>
Script files | <code> /opt/NMAP/share/NMAP/scripts/<name>.nse </code>

> **MASSCAN**

Example Command | <code> nohup masscan -p<ports> --rate 500 -vv --includefile <host list> --output-format list --output-filename <outputfile> > output.txt & </code>




