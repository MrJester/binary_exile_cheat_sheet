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

> **Installing Kali**

{% highlight bash %}
#!/bin/bash
echo Change Password
passwd
echo Update
apt-get update && apt-get -y upgrade
echo Setting up Metasploit Database to Start on Boot
service postgresql start
update-rc.d postgresql enable
echo Installing gedit
apt-get -y install gedit
echo Changing Host Name
echo Workstation > /etc/hostname
sed -i 's/kali/Workstation/g' /etc/hosts
echo Turn on Metasploit Logging
echo “spool /root/msf_console.log” > /root/.msf5/msfconsole.rc
echo Lists of fuzzing parameters, paswords, ect to /opt/SecLists
git clone https://github.com/danielmiessler/SecLists.git /opt/SecLists
echo Installs Discover
git clone https://github.com/leebaird/discover.git /opt/discover && /opt/discover/update.sh
echo Installing Pure FTP
apt-get install pure-ftpd 
groupadd ftpgroup useradd -g ftpgroup -d /dev/null -s /etc ftpuser 
pure-pw useradd ftpuser -u ftpuser -d /ftphome 
pure-pw mkdb 
cd /etc/pure-ftpd/auth/ 
ln -s ../conf/PureDB 60pdb 
mkdir -p /ftphome 
chown -R ftpuser:ftpgroup /ftphome/ 
/etc/init.d/pure-ftpd restart 
chmod 755 setup-ftp 
./setup-ftp 
echo Installing Wappalyzer
wget https://addons.mozilla.org/firefox/downloads/latest/wappalyzer/addon-10229-latest.xpi && firefox -install-global-extension addon-10229-latest.xpi && rm *.xpi
echo Installing foxyproxy
wget https://addons.mozilla.org/firefox/downloads/latest/foxyproxy-standard/addon-2464-latest.xpi && firefox -install-global-extension addon-2464-latest.xpi && rm *.xpi
echo Installing Developer Toolbar
wget https://addons.mozilla.org/firefox/downloads/latest/web-developer/addon-60-latest.xpi && firefox -install-global-extension addon-60-latest.xpi && rm *.xpi
{% endhighlight %}


