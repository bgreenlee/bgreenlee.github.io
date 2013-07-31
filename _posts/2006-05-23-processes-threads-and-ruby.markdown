--- 
wordpress_id: 53
title: Processes, Threads, and Ruby
date: 2006-05-23 13:50:40 +00:00
wordpress_url: http://footle.org/blog/?p=53
layout: post
---
While researching the best way to handle calling an external program from Ruby (and capturing stdout & stderr), I came across this post, which is a good review of how processes and threads work:

<a href="http://www.ruby-forum.com/topic/65155#75363">http://www.ruby-forum.com/topic/65155#75363</a>

I still haven't figured out exactly how I'm going to do this, but I'll post it here when I figure it out. Ruby has a few different ways of opening and communicating with processes, but all seem to be lacking in some way or another. IO.popen lets you write to the process' stdin, and read from its stdout, but you can't get stderr without jumping through serious hoops (like redirecting stderr to a file and then reading the file...ugh). Open3.popen3 (brilliant naming) gives you stdin, stdout, and stderr, but the subprocess runs as a grandchild, so there seems to be no way to wait for it to finish.

