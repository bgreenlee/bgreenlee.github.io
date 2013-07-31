--- 
wordpress_id: 77
title: I Get No Spam
date: 2007-08-14 18:26:00 +00:00
wordpress_url: http://blog.footle.org/2007/08/14/i-get-no-spam/
layout: post
---
After hearing a <a href="http://twitter.com/indirect/statuses/206360792">couple</a> <a href="http://twitter.com/coda/statuses/206365502">complaints</a> from friends about the amount of spam they're getting, I decided to take a quick look to see where I stood. At the risk of setting myself up, I should say that I get no spam (channeling <a href="http://www.dvorak.org/blog/?p=2595">John C. Dvorak</a>). Or at least very little--maybe one every other day gets through to my inbox. 

I think the trick is just to have multiple levels of filtering. I use a Gmail address for most online transactions (for online transactions that I really don't care about--if I'm doing a one-time registration just to get a trial key, for example--I use my Yahoo address, which is nothing but a spam bucket). My Gmail address gets forwarded to my personal email address (@footle.org), which has <a href="http://spamassassin.apache.org/">SpamAssassin</a> running on the server. If it makes it past SA to Mail.app, it gets inspected by <a href="http://c-command.com/spamsieve/">SpamSieve</a>, an awesome Bayesian spam filter for OS X mail clients.

Anyway, my stats for the last 24 hours:

* Server-level filters (spam caught):
    * SpamAssassin: 281
    * Gmail: 32
* Client-level filter (spam caught):
    * SpamSieve: 17
* Spam getting to my inbox: 0
* Non-spam messages: 52
* False positives: 1 (just [Flavorpill](a href="http://flavorpill.net/), so not a big deal)

(Wow...330 pieces of spam in a single day. That seems way worse than when I last checked.)
