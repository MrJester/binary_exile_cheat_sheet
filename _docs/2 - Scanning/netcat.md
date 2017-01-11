---
title: Netcat
category: Scanning
order: 5
---

> **Netcat**

Banner Grabbing | {% highlight bash %} nc [ip] [remote_port] <connection string><HEAD / HTTP/1.0> {% endhighlight %}
Grab a range of banners | {% highlight bash %}echo "" &#124; nc -v -n -w1 <-r> [targetIP] [port-range] {% endhighlight %}
Obtain User Agent Strign | {% highlight bash %} nc -v -l -p [loacl_port] {% endhighlight %}
Service-is-alive Heartbeat | {% highlight bash %} while (true); do nc -vv -z -w3 [target_IP] [target_port} > /dev/null && echo-e "\x07"; sleep 1; done {% endhighlight %}
Service-is-Dead | {% highlight bash %} while &#96;nc -vv -z -w3 [target_IP] [target_port] > /ddev/null&#96;  ; do echo "Sevice is ok"; sleep 1; done; echo "Service is dead"; echo -e "\x07" {% endhighlight %}




