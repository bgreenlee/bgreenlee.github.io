---
title: "Automating STOP to End"
date: 2024-03-31 17:48:17 -0700
layout: post
tags: ios shortcuts automation
---

My friends Sunah Suh, [Anthony Hersey](https://github.com/stormsweeper), and I finally figured out the magic incantations needed to automate sending "STOP" to political text messages. Setting it up is completely non-obvious, so I'll walk through it step-by-step.

Note that this is for iOS only. If someone with an Android phone wants to write up their process, I'd be happy to link to it.

<p class="img-right">
<img alt="Shortcuts home screen" src="/public/images/stop-to-end/s2e01.jpeg">
First, open the Shortcuts app.
</p>

<p class="img-right">
<img alt="Automation home screen" src="/public/images/stop-to-end/s2e02.jpeg">
Hit the "Automation" tab at the bottom.
</p>

<p class="img-right">
<img alt="Create automation search page" src="/public/images/stop-to-end/s2e03.jpeg">
Hit the plus (+) in the top right corner. Start typing "Message" in the search bar, and select Message.
</p>

<p class="img-right">
<img alt="Automation When screen" src="/public/images/stop-to-end/s2e04.jpeg">
On the When page, hit Choose next to Message Contains.
</p>

<p class="img-right">
<img alt="Automation When Message Contains screen" src="/public/images/stop-to-end/s2e05.jpeg">
Type in the text to look for, such as "STOP to end". It appears to be case-insensitive.
</p>

<p class="img-right">
<img alt="Automation When screen selecting Run Immediately" src="/public/images/stop-to-end/s2e06.jpeg">
Hit Done and the back on the When page, select Run Immediately. Then hit Next in the top-right corner.
</p>

<p class="img-right">
<img alt="When I Get a Message Containing... screen" src="/public/images/stop-to-end/s2e07.jpeg">
Now select New Blank Automation.
</p>

<p class="img-right">
<img alt="When I Get a Message Containing... Add Action screen" src="/public/images/stop-to-end/s2e08.jpeg">
Under Next Action Suggestions, you should see Send Message. (If not, hit Add Action and search for "Send Message".) Click Send Message.
</p>

<p class="img-right">
<img alt="Send Message Action screen" src="/public/images/stop-to-end/s2e09.jpeg">
Click on the "Message" field and type "STOP".
</p>

<p class="img-right">
<img alt="Send Message Recipients Shortcut Input" src="/public/images/stop-to-end/s2e10.jpeg">
Now comes the first tricky part. Hold down on Recipients (long press) and select Shortcut Input. (If instead it pops open a "To:" page, you didn't hold down long enough. Cancel and try again.)
</p>

<p class="img-right">
<img alt="Send Message Recipients Shortcut Input Sender" src="/public/images/stop-to-end/s2e11.jpeg">
Now the next tricky bit. Tap the "Shortcut Input" field and select Sender, then hit Done.
</p>

<p class="img-right">
That's it! Now repeat this for different variations of "STOP to...." ("STOP to quit", "STOP2END", etc.)
</p>
