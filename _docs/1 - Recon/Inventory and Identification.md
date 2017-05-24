---
title: Inventory and Identification
category: Recon
order: 0
---

> **Inventory and Identification** 

* Interview Administrator on Software Versions
* Recon Phase: Document Metadata (see metadata)
* Recon Phase: ReconNG DNS Cache Snoop (see recon -> Recon-NG)
* Scanning Phase Information
* Phishing page to identify browser

> **Windows Inventory**


{% highlight bash %}
dir /s "c:\Program Files" > inventory.txt<br>
dir /s "c:\Program Files (x86)" >> inventory.txt<br>
ipconfig /displaydns
{% endhighlight %}
