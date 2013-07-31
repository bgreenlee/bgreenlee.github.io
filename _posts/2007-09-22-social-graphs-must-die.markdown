--- 
wordpress_id: 78
title: Social Graphs Must Die
date: 2007-09-22 23:24:15 +00:00
wordpress_url: http://blog.footle.org/2007/09/22/social-graphs-must-die/
layout: post
---
<a href="http://errtheblog.com/">Chris Wanstrath</a> made <a href="http://twitter.com/defunkt/statuses/286921322">a perfectly reasonable request</a> earlier today, so I came up with the following <a href="https://addons.mozilla.org/en-US/firefox/addon/748">Greasemonkey</a> script:

{% highlight javascript %}
// ==UserScript==
// @name           Social Graphs Must Die
// @namespace      Footle
// @description    Redact all mentions of "social graphs";
// ==/UserScript==
document.body.innerHTML = document.body.innerHTML.replace(/(social\s+graph(s?))/ig,"<span style='background:#000;color:#000'>$1</span>");
{% endhighlight %}

Actually closing a tab (that wasn't opened by Javascript) in Firefox 2 isn't possible AFAICT, but this is more fun anyway.
