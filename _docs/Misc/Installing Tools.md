---
title: Install Tools
category: Misc
order: 1
---
> <b> Installing Metasploit </b>

{% highlight bash %}
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && \
  chmod 755 msfinstall && \
  ./msfinstall
{% endhighlight %}

