---
title: Web Services
category: Web Application
order: 80
---

> **Obtain Definition **

WADL | Rest
WSDL | SOAP

> **Example SOAP***

Focus on input in between the tags:

{% highlight xml %}
<?xml version="1.0" encoding="utf-8?>
<soap:Envelope
xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:xsd="http://www.w3.org/2001/XMLSchema>
	<soap:Boady>
		<GetURLIP xmlns="http://example.com/webservices/">
		<EnterURL>www.owasp.org</EnterURL>
		</GetURLIP>
	</soap:body>
</soap:Envelope>
{% endhighlight %}


> **SOAP Tools**

* Burp WSDler and WSDL Wizard
* SoapUI 
- File new project
- Right click on project and add WSDL or WADL
