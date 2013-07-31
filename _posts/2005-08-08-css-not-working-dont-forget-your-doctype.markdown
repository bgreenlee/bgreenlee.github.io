--- 
wordpress_id: 19
title: CSS Not Working? Don't Forget Your Doctype
date: 2005-08-08 18:54:22 +00:00
wordpress_url: http://footle.org/blog/?p=19
layout: post
---
<p>I just spend *hours* trying to get some CSS working only to discover that the reason it wasn't working was because I didn't have the DOCTYPE declared at the top of my html:</p>

{% highlight html %}
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
         "DTD/xhtml1-strict.dtd">
{% endhighlight %}



