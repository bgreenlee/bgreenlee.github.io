--- 
wordpress_id: 60
title: "Campfire Hacks: Uploading Screenshots with Quicksilver and Pyro"
date: 2007-02-04 14:39:02 +00:00
wordpress_url: http://blog.footle.org/?p=60
layout: post
---
<p>We live in <a href="http://campfirenow.com/">Campfire</a> at <a href="http://www.wesabe.com">Wesabe</a>, so naturally we've developed a number of tools and hacks around it. One of these days I'll get around to writing about the Campfire bot framework we've developed, but right now I'll start with something simpler: a way to upload screenshots to Campfire with only a few keystrokes (riffing of my colleague <a href="http://blog.codahale.com">Coda Hale</a>'s <a href="http://blog.codahale.com/2007/01/22/send-as-im-adium-quicksilver/">desire to eliminate</a> <a href="http://blog.codahale.com/2007/01/15/tweet-twitter-quicksilver/">the need for a mouse</a>). Note that this is for Mac users only, so if you're one of the poor souls who hasn't seen the light yet, you can stop reading now.</p>

<p>Two prerequisites (other than a Mac): <a href="http://quicksilver.blacktree.com/">Quicksilver</a> and <a href="http://www.karppinen.fi/pyro/">Pyro</a>. Quicksilver should be familiar to most Mac users already. It is an incredibly powerful application that can be quite obtuse but can save you gobs of time if you learn how to use it. Pyro is a client for Campfire. Why do you need a client for a browser-based chat app? Getting message notifications in your dock is one reason, but the biggest reason is that there seems to be a memory leak in Firefox that causes it to grind to a halt if you've had Campfire up for too long. But I digress.</p>

<p>You first need to enable the Screen Capture Actions plugin in Quicksilver (go to Plugins -&gt; Recommended, and check Screen Capture Actions). Then to go Catalog -&gt; Quicksilver and make sure that Internal Commands is checked). This should give you Capture Region, Window, and Screen commands.</p>

<p>Next, the secret sauce: a bit of AppleScript that uploads a file to Campfire via Pyro:</p>

{% highlight applescript %}
on open theFile
  tell application "Pyro"
    upload theFile to room "[your campfire room name]" in campfire "[your campfire].campfirenow.com"
  end tell
end open
{% endhighlight %}

<br />
<p>Paste that into Script Editor and save it as <code>PyroUpload</code> in <code>~/Library/Application Support/Quicksilver/Actions</code> (if this folder doesn't exist, create it), and restart Quicksilver.<br /><br />Now you can upload a screenshot with just Cmd - Space - [Capture Window|Region|Screen] - Return - [take your screenshot] - PY - Return.</p>

<p>Have fun!</p>
