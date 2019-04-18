---
title: Linux Troublehsooting
category: Troubleshooting
order: 0
---

> **OpenSSH**
When getting error message:<br>
packet_write_wait: Connection to <HOSTNAME/IP> port <NUMBER>: Broken pipe

Within your ssh client configuration file /etc/ssh/ssh_config
Add the following

<pre><code>Host *
    ServerAliveInterval 120
    ServerAliveCountMax 5
    TCPKeepAlive yes
    IPQoS throughput</code></pre>