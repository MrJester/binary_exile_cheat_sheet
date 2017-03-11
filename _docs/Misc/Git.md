---
title: Jekyll and Git 
category: Misc
order: 2
---
> <b> Generating SSH Key </b>

{% highlight bash %}
$ ssh-keygen -t rsa -C "github-user1" -f "id_rsa"
$ chmod 400 github-user1"
$ git config --global user.name "BinaryExile"
$ git config user.email "binaryexile@noreply.noreply"
$ vi ~/.ssh/config
{% endhighlight %}

{% highlight bash %}
Host github.com-user1
    HostName github.com
    User git
    IdentityFile ~/.ssh/github-user1
{% endhighlight %}

{% highlight bash %}
$ chmod 400 ~/.ssh/config
{% endhighlight %}

>Cloning, Pulling, Commiting, and Pushing

{% highlight bash %}
$ git clone git@github.com:BinaryExile/BinaryExile.github.io.git CheatSheets_BinaryExile
$ git pull
$ git git commit -a
$ git add 
$ git status
$ git add "document to add" 
$ git push origin master
$ git add . && git commit -a -m "updated content" && git push
{% endhighlight %}

>Modifying Comment History

{% highlight bash %}
$ git rebase -i HEAD~30
$ git git commit -a
{% endhighlight %}

Change **pick** with **reword** 

{% highlight bash %}
$ git push --force 
{% endhighlight %}

> Cheat Sheets

* [Jekyll](https://sourceforge.net/p/jekyllc/bugs/markdown_syntax#md_ex_tables)

* [Git](https://www.git-tower.com/blog/git-cheat-sheet/)

* [Syntax Highlighting](https://github.com/jneen/rouge/wiki/List-of-supported-languages-and-lexers)
