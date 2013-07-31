--- 
wordpress_id: 103
title: MySQL-Style Output for the Rails Console
date: 2009-03-01 18:42:47 +00:00
wordpress_url: http://blog.footle.org/?p=103
layout: post
---
While poking around the database in my rails console, I often found myself jumping to the mysql console just so I could get an easier-to-digest view of the data. I finally had enough of this silliness and wrote up a quick function to dump of set of ActiveRecord objects in the mysql report format.

{% highlight irb %}
>> report(records, :id, :amount, :created_at)
+------+-----------+--------------------------------+
| id   | amount    | created_at                     |
+------+-----------+--------------------------------+
| 8301 | $12.40    | Sat Feb 28 09:20:47 -0800 2009 |
| 6060 | $39.62    | Sun Feb 15 14:45:38 -0800 2009 |
| 6061 | $167.52   | Sun Feb 15 14:45:38 -0800 2009 |
| 6067 | $12.00    | Sun Feb 15 14:45:40 -0800 2009 |
| 6059 | $1,000.00 | Sun Feb 15 14:45:38 -0800 2009 |
+------+-----------+--------------------------------+
5 rows in set
{% endhighlight %}

<a href="http://gist.github.com/72234">Grab it from GitHub</a> and stick it in your <code>.irbrc</code>.
