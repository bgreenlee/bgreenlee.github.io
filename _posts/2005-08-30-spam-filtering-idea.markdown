--- 
wordpress_id: 23
title: Spam Filtering Idea
date: 2005-08-30 10:13:36 +00:00
wordpress_url: http://footle.org/blog/?p=23
layout: post
---
<p>I share a hosted server with a few friends. We host a number of sites (including this one) and our own email. We have SpamAssassin set up and tuned pretty nicely (thanks, Jelo). Unfortunately, it's killing our server. SpamAssassin can suck up a lot of resources. Ideally we'd have a server dedicated to processing mail, but we can't afford that.</p>

<p>So here's my idea: a spam filtering cluster. Have a bunch of [trusted] home-based servers set up with SpamAssassin and dynamic DNS. A mail comes into the main mailserver and if it isn't on a whitelist, it gets shipped out to one of the filter machines. The filter machine runs SA on it and lets the main server know the result (it doesn't need to sent the mail back, so no worries about low upload speeds). Emails over a certain size would probably just be processed on the main server to avoid the bandwidth and time required to send it out (large emails are rarely spam anyway). Finally, in order for the Bayesian filtering to function correctly, you'd have to periodically sync the data from the Bayesian learner out to the filter machines.</p>

<p>I've looked at SpamAssassin a bit and I think it wouldn't be too hard. SA comes with a client and a server, spamc and spamd. Incoming mail gets piped through spamc, which takes care of shipping it off to spamd for processing. Spamd can live on any other host. It can also just report back whether the email was spam or not, rather than returning the entire email. So the only two things left to do are:</p>
<ol>
  <li>Write a small client (spamb?) that maintains a list of hosts running spamd. When a message comes in, pick the next hostname in the list, and pass it on to spamc. Spamc sends it to the appropriate spamd host, and receives the response--either yes/no or in our case, the full SA report, which gets attached to the message headers. If we get the full message back, that means that spamc timed out trying to contact that spamd host. In this case, mark that host as being down, along with a timestamp, and take it out of the rotation for a certain period of time.</li>
<li>A way to distribute the users' bayesian data files and prefs to the remote systems. Apparently spamd can read user from a SQL database, although I haven't looked into it to see if the bayesian learner data can be stored in a database. If so, that's an easy solution to the problem. Otherwise, you could just write a script that checks for changes in any of the user files and if it sees them, rsyncs them to each of the hosts.</li>
</ol>

<p>I'll let you know what happens if I get around to trying this out.</p>



