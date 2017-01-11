---
title: Netcat
category: Scanning
order: 5
---

> **Netcat**

Banner Grabbing | {% highlight bash %} nc [ip] [remote_port] <connection string><HEAD / HTTP/1.0> {% endhighlight %}
Grab a range of banners | echo "" &#124; nc -v -n -w1 <-r> [targetIP] [port-range]
Obtain User Agent Strign | nc -v -l -p [loacl_port]
Service-is-alive Heartbeat | while (true); do nc -vv -z -w3 [target_IP] [target_port} > /dev/null && echo-e "\x07"; sleep 1; done
Service-is-Dead | while &#96;nc -vv -z -w3 [target_IP] [target_port] > /ddev/null&#96;  ; do echo "Sevice is ok"; sleep 1; done; echo "Service is dead"; echo -e "\x07"




