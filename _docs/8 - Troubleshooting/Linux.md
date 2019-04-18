---
title: Linux Troublehsooting
category: Troubleshooting
order: 0
---

> **OpenSSH**
When getting error message:
packet_write_wait: Connection to <HOSTNAME/IP> port <NUMBER>: Broken pipe

Within your ssh client configuration file /etc/ssh/ssh_config
Add the following

<code><pre>Host *
    ServerAliveInterval 120
    ServerAliveCountMax 5
    TCPKeepAlive yes
    IPQoS throughput</pre></code>