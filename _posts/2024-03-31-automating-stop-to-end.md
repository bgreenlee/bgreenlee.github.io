---
title: "Automating STOP to End"
date: 2024-03-31 17:48:17 -0700
layout: post
tags: ios shortcuts automation
---

My friends Sunah Suh, [Anthony Hersey](https://github.com/stormsweeper), and I finally figured out the magic incantations needed to automate sending "STOP" to political text messages. Setting it up is completely non-obvious, so I'll walk through it step-by-step.

Note that this is for iOS only. If someone with an Android phone wants to write up their process, I'd be happy to link to it.

Also, this was done on iOS 17.4. Surely this process will change some day. If you notice something is off, <a href="mailto:brad@footle.org">let me know</a> and I'll update it.

<p class="img-right">
<img alt="Shortcuts home screen" src="/public/images/stop-to-end/s2e01.jpeg">
First, open the Shortcuts app and tap the <b>Automation</b> tab at the bottom.
</p>

<p class="img-right">
<img alt="Automation home screen" src="/public/images/stop-to-end/s2e02.jpeg">
Tap the <b>plus (+)</b> in the top right corner.
</p>

<p class="img-right">
<img alt="Create automation search page" src="/public/images/stop-to-end/s2e03.jpeg">
Start typing "Message" in the search bar, and select <b>Message</b>.
</p>

<p class="img-right">
<img alt="Automation When screen" src="/public/images/stop-to-end/s2e04.jpeg">
On the When page, hit <b>Choose</b> next to <b>Message Contains</b>.
</p>

<p class="img-right">
<img alt="Automation When Message Contains screen" src="/public/images/stop-to-end/s2e05.jpeg">
Type in the text to look for, such as "STOP to end". It appears to be case-insensitive.
</p>

<p class="img-right">
<img alt="Automation When screen selecting Run Immediately" src="/public/images/stop-to-end/s2e06.jpeg">
Hit <b>Done</b> and back on the When page, select <b>Run Immediately</b>. Then tap <b>Next</b> in the top-right corner.
</p>

<p class="img-right">
<img alt="When I Get a Message Containing... screen" src="/public/images/stop-to-end/s2e07.jpeg">
Now select <b>New Blank Automation</b>.
</p>

<p class="img-right">
<img alt="When I Get a Message Containing... Add Action screen" src="/public/images/stop-to-end/s2e08.jpeg">
Under <b>Next Action Suggestions</b>, you should see <b>Send Message</b>. (If not, hit <b>Add Action</b> and search for "Send Message".) Tap <b>Send Message</b>.
</p>

<p class="img-right">
<img alt="Send Message Action screen" src="/public/images/stop-to-end/s2e09.jpeg">
Tap on the <b>Message</b> field and type "STOP".
</p>

<p class="img-right">
<img alt="Send Message Recipients Shortcut Input" src="/public/images/stop-to-end/s2e10.jpeg">
Now comes the first tricky part. Hold down (long press) on <b>Recipients</b> and select <b>Shortcut Input</b>. (If instead it pops open a "To:" page, you didn't hold down long enough. Cancel and try again.)
</p>

<p class="img-right">
<img alt="Send Message Recipients Shortcut Input Sender" src="/public/images/stop-to-end/s2e11.jpeg">
Now the next tricky bit. Tap the <b>Shortcut Input</b> field and select <b>Sender</b>, then hit <b>Done</b>.
</p>

<p class="img-right">
That's it! Now repeat this for different variations of "STOP to...." ("STOP to quit", "STOP2END", etc.)
</p>
