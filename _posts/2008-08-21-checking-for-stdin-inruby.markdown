--- 
wordpress_id: 88
title: Checking for STDIN in ruby
date: 2008-08-21 17:24:06 +00:00
wordpress_url: http://blog.footle.org/?p=88
layout: post
---
A colleague was asking today how he could see if there's data waiting on <code>STDIN</code> in ruby (on Linux). On OS X, this is pretty straightforward:

{% highlight ruby %}
if $stdin.stat.size > 0
  puts "got something: #{STDIN.read}"
else
  puts "nada"
end
{% endhighlight %}

That doesn't work in Linux, though. After some digging, I found <a href="http://blade.nagaokaut.ac.jp/cgi-bin/scat.rb/ruby/ruby-talk/186313">this post</a>, which lead to:

{% highlight ruby %}
require 'fcntl'

flags = STDIN.fcntl(Fcntl::F_GETFL, 0)
flags |= Fcntl::O_NONBLOCK
STDIN.fcntl(Fcntl::F_SETFL, flags)

begin
  puts "got something: #{STDIN.read}"
rescue Errno::EAGAIN
  puts "nada"
end
{% endhighlight %}

Which works on both platforms, but is (a) ugly (catching an exception), and (b) requires you to actually try to read from <code>STDIN</code>.

In playing around with that, though, I noticed that <code>STDIN.fcntl(Fcntl::F_GETFL, 0)</code> returned <code>0</code> if there was something on <code>STDIN</code>, and something non-zero (<code>2</code> in OSX and <code>32770</code> in Linux) if <code>STDIN</code> was empty. So now the code is simple again:

{% highlight ruby %}
require 'fcntl'

if STDIN.fcntl(Fcntl::F_GETFL, 0) == 0
  puts "got something: #{STDIN.read}"
else
  puts "nada"
end
{% endhighlight %}

I'm not sure how reliable that is on other platforms (it won't work on Windows&mdash;fcntl is a Unix-y thing). If I'm understanding fcntl.h correctly, a return value of 2/32770 (0x8002) seems to indicate that the io stream in question is open for writing. I'm not sure if that is intended or just a side-effect. Anyone know?
