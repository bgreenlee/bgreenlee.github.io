--- 
wordpress_id: 73
title: Reloading Ruby Classes in Rails
date: 2007-05-13 19:58:54 +00:00
wordpress_url: http://blog.footle.org/2007/05/13/reloading-ruby-classes-in-rails/
layout: post
---
I ran across a bug in Date::Format today, and after spending a few hours hacking away at a fix (the date/format.rb code is *uuuuugly* and *sloooow*...someone should really rewrite that. Better yet, rewrite it in C), I thought I'd submit a patch. So I grabbed the <a href="http://www.ruby-lang.org/en/community/ruby-core/">ruby_1_8 branch</a> and lo and behold, my issue had already been fixed!

So the question now was how to monkeypatch the entire Date::Format module? Simply <code>require</code>-ing it as a plugin doesn't work, since Date::Format is already loaded at that point. The trick then is to use <code>load</code> instead of require.

First, I tried this:

{% highlight ruby %}
load 'date/format.rb'
{% endhighlight %}

(note: load needs the actual filename; it doesn't have the magic that require does) but that gave me an error:

{% highlight irb %}
in `load': wrong number of arguments (1 for 0) (ArgumentError)
{% endhighlight %}

Turns out Rails::Plugin::Loader defines its own <code>load</code>, so:

{% highlight ruby %}
Kernel::load 'date/format.rb'
{% endhighlight %}

et voila!
