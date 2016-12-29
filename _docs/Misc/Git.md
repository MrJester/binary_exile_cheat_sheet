---
title: Git 
category: Misc
order: 1
---

Using git and github

>Generating SSH Key
{% highlight bash %}
&#35 ssh-keygen -t rsa -C "github-user1" -f "github-user1"
&#35 chmod 400 github-user1"
&#35 git config --global user.name "BinaryExile"
&#35 git config user.email "binaryexile@noreply.noreply"
&#35 vi ~/.ssh/config
{% endhighlight %}

{% highlight bash %}
Host github.com-user1
    HostName github.com
    User git
    IdentityFile ~/.ssh/github-user1
{% endhighlight %}

{% highlight bash %}
&#35 chmod 400 ~/.ssh/config
{% endhighlight %}

>Cloning, Commiting, and Pushing
{% highlight bash %}
&#35 git clone git@github.com:BinaryExile/BinaryExile.github.io.git CheatSheets_BinaryExile
&#35 git git commit -a
&#35git add 
&#35 git status
&#35git add "document to add" 
&#35git push origin master
{% endhighlight %}

>Modifying Comment History
{% highlight bash %}
&#35 git rebase -i HEAD~30
&#35 git git commit -a
{% endhighlight %}

Change **pick** with **reword** 

{% highlight bash %}
&#35 git push --force 
{% endhighlight %}
