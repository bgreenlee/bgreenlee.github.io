--- 
wordpress_id: 54
title: HTTP Authorization with Apache/FastCGI
date: 2006-05-24 17:56:12 +00:00
wordpress_url: http://footle.org/blog/?p=54
layout: post
---
It took me forever to figure this one out, but if you want HTTP Authentication to work with Apache 2 and mod_fastcgi, you need this in your apache conf file:

{% highlight apache %}
FastCgiConfig -pass-header Authorization
{% endhighlight %}

FastCGI doesn't pass the Authorization header by default for some reason.