--- 
wordpress_id: 2
title: Always check your logs
date: 2005-07-08 20:30:50 +00:00
wordpress_url: http://footle.org/wordpress/?p=2
layout: post
---
Brad's #1 rule of debugging web apps: always check your log files. All of them.

A friend of mine just ran into a problem where a php file upload script that had been working fine for months suddenly started barfing on large files. He was getting a "partial upload" error. I told him to check his web server error logs and his system message log. He did the former, and after hours of pounding his head against the wall, I finally went in and poked around myself. I actually found the problem before looking in the log files, but there it was in <code>/var/log/messages</code>:

{% highlight text %}
Jul 7 15:27:42 xxxx /kernel: pid 46891 (httpd), uid 80 on /var: file system full
Jul 7 15:30:02 xxxx /kernel: pid 46875 (httpd), uid 80 on /var: file system full
Jul 7 15:49:07 xxxx /kernel: pid 46872 (httpd), uid 80 on /var: file system full
{% endhighlight %}

The temp directory that php was using for file uploads was <code>/var/tmp</code>, and <code>/var</code> only had 216K left. This is why small file uploads were working, but larger ones weren't. (As an aside, be careful of what you're storing in <code>/var</code>. On some systems, log files are kept in <code>/var/log</code>, and /var is often set up as a separate partition, and often relatively small at that. In his case, the whole partition was only 252M, and <code>/var/log</code> was eating up 195M of that...and growing. We moved <code>/var/log</code> to <code>/usr/var/log</code> and created a symlink.)
