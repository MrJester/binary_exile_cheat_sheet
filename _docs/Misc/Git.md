---
title: Git 
category: Misc
order: 1
---

# Generating SSH Key
{% highlight bash %}
# ssh-keygen -t rsa -C "github-user1" -f "github-user1"
# chmod 400 github-user1"
# git config --global user.name "BinaryExile"
# git config user.email "binaryexile@noreply.noreply"
# vi ~/.ssh/config
{% endhighlight %}

{% highlight bash %}
Host github.com-user1
    HostName github.com
    User git
    IdentityFile ~/.ssh/github-user1
{% endhighlight %}

{% highlight bash %}
# chmod 400 ~/.ssh/config
{% endhighlight %}

>Cloning, Commiting, and Pushing
{% highlight bash %}
# git clone git@github.com:BinaryExile/BinaryExile.github.io.git CheatSheets_BinaryExile
# git git commit -a
# git add 
# git status
# git add "document to add" 
# git push origin master
{% endhighlight %}

>Modifying Comment History
{% highlight bash %}
# git rebase -i HEAD~30
# git git commit -a
{% endhighlight %}

Change **pick** with **reword** 

{% highlight bash %}
# git push --force 
{% endhighlight %}
