---
title: Netcat
category: Scanning
order: 5
---

> **Netcat**

Banner Grabbing | <code> nc [ip] [remote_port] &#60;connection string&#62;&#60;HEAD&#62;/HTTP/1.0&#62;</code>
Grab a range of banners | <code> echo "" &#124; nc -v -n -w1 [-r] [targetIP] [port-range] </code>
Obtain User Agent Strign | <code> nc -v -l -p [loacl_port] </code>
Service-is-alive Heartbeat | <code> while (true); do nc -vv -z -w3 [target_IP] [target_port} > /dev/null && echo-e "\x07"; sleep 1; done </code>
Service-is-Dead | <code> while &#96;nc -vv -z -w3 [target_IP] [target_port] > /dev/null&#96;; do echo "Sevice is ok"; sleep 1; done; echo "Service is dead"; echo -e "\x07" </code>




