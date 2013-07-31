--- 
wordpress_id: 239
title: Installing Ruby 1.9.2 via rvm on OS X
date: 2010-07-29 20:34:10 +00:00
wordpress_url: http://blog.footle.org/?p=239
layout: post
---
It took me a bit of digging to figure this out (it kept failing with readline errors, and then iconv went missing), so I thought I'd share:

{% highlight bash %}
rvm package install readline
rvm package install iconv
rvm install 1.9.2 -C --enable-shared,--with-iconv-dir=$HOME/.rvm/usr,\
--with-readline-dir=$HOME/.rvm/usr
{% endhighlight %}

**Update:** The `package` command has been renamed `pkg` in newer versions of rvm.
