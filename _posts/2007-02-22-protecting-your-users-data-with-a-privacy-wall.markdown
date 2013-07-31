--- 
wordpress_id: 68
title: Protecting Your Users' Data with a Privacy Wall
date: 2007-02-22 10:40:08 +00:00
wordpress_url: http://blog.footle.org/2007/02/22/protecting-your-users-data-with-a-privacy-wall/
layout: post
---
<p><div style="float:right;margin:0.5em 0.5em 0.5em 0.5em;"><img alt="Just Another Brick In The Wall? by Iain Cuthbertson" src="http://farm1.static.flickr.com/32/64441215_1e52c533a5_m.jpg"/><br />
<span style="font-size:75%;color:#999">Just Another Brick In The Wall?<br />by <a href="http://www.flickr.com/photos/bigcuthy/">Iain Cuthbertson</a></span></div>

We deal with a lot of very private data at <a href="http://www.wesabe.com">Wesabe</a>, so security and privacy are our top concerns. In this post I will describe one of our primary means for assuring privacy, a technique that is general enough that any site can use it.  Our creative name for this technique is the <strong>privacy wall</strong>. Later, I'll go on to tell you ways to hack the wall, just so you don't get too comfortable.</p>

<h3>The Privacy Wall</h3>

<p>The idea is simple: don't have any direct links in your database between your users' "public" data and their private data. Instead of linking tables directly via a foreign key, use a cryptographic hash <a href="#footnote_1">[1]</a> that is based on at least one piece of data that only the user knows&mdash;such as their password. The user's private data can be looked up when the user logs in, but otherwise it is completely anonymous. Let's go through a simple example.</p>

<p>Let's say we're designing an application that lets members keep a list of their deepest, darkest secrets. We need a database with at least two tables: 'users' and 'secrets'. The first pass database model looks like this:</p>

<img src="/images/standard-model.png" alt="Standard Model" height="111" width="348"/>

<p style="clear:both">The problem with this schema is that anyone with access to the database can easily find out all the secrets of a given user. With one small change, however, we can make this extremely difficult, if not impossible:</p>

<img src="/images/privacy-wall2.png" alt="Privacy Wall" height="111" width="348"/>

<p style="clear:both">The special sauce is the 'secret_key', which is nothing more than a cryptographic hash of the user's username and their password <a href="#footnote_2">[2]</a>. When the user logs in, we can generate the hash and store it in the session <a href="#footnote_3">[3]</a>. Whenever we need to query the user's secrets, we use that key to look them up instead of the user id. Now, if some baddie gets ahold of the database, they will still be able to read everyone's secrets, but they won't know which secret belongs to which user, and there's no way to look up the secrets of a given user.</p>

<p><strong>Update:</strong> A commenter on the Wesabe blog brought up the important point of what you do if the user forgets their password. The recovery method we came up with was to store a copy of their secret key, encrypted with the answers to their security questions (which aren't stored anywhere in our database, of course). Assuming that the user hasn't forgotten those as well, you can easily find their account data and "move it over" when they reset their password (don't forget to update the encrypted secret key); if they do forget them, well, there's a problem.</p>

<h3>Attacking the Wall</h3>

<p>I mentioned earlier that you store the secret key in the user's session. If you're storing your session data in the database and your db is hacked, any users that are logged in (or whose sessions haven't yet be deleted) can be compromised. The same is true if sessions are stored on the filesystem. Keeping session data in memory is better, although it is still hackable (the swapfile is one obvious target). However you're storing your session data, keeping your sessions reasonably short and deleting them when they expire is wise. You could also store the secret key separately in a cookie on the user's computer, although then you'd better make damn sure you don't have any <a href="http://en.wikipedia.org/wiki/XSS">cross-site scripting (XSS)</a> vulnerabilities that would allow a hacker to harvest your user's cookies.</p>
	
<p>Other holes can be found if your system is sufficiently complex and an attacker can find a path from User to Secret through other tables in the database, so it's important to trace out those paths and make sure that the secret key is used somewhere in each chain.</p>

<p>A harder problem to solve is when the secrets themselves may contain enough information to identify the user, and with the above scheme, if one secret is traced back to a user, all of that user's secrets are compromised. It might not be possible or practical to scrub or encrypt the data, but you can limit the damage of a secret being compromised. My colleague and security guru <a href="http://www.emerose.com">Sam Quiqley</a> suggests the following as an extra layer of security: add a counter to the data being hashed to generate the secret key:</p>

{% highlight text %}
secret key 1 = Hash(salt + password + '1')
secret key 2 = Hash(salt + password + '2')
...
secret key n = Hash(salt + password + '&lt;n&gt;')
{% endhighlight %}

<p>Getting a list of all the secrets for a given user when they log in is going to be a lot less efficient, of course; you have to keep generating hashes and doing queries until no secret with that hash is found, and deleting secrets may require special handling. But it may be a small price to pay for the extra privacy.</p>

<p>Finally, log files can be a gold mine for attackers. There's a very good chance you're logging queries, debug statements, or exception reports that link users to their keys or directly to their secrets. You should scrub any identifying information before it gets written to the log file.</p>

<h3>So That's It, Right?</h3>

<p>The privacy wall is far from a silver bullet. Privacy and security are hard&mdash;really hard&mdash;particularly so if your app is taking private data and extracting information out of it for public consumption, like we are at Wesabe. The privacy wall is one of a number of methods we're using to insure that our users' private data stays that way. If you're lucky enough to be going to ETech next month, definitely check out <a href="https://www.wesabe.com/page/founders#marc">Marc's</a> session on <a href="http://conferences.oreillynet.com/cs/et2007/view/e_sess/10492">Super Ninja Privacy Techniques for Web App Developers</a>.</p>

<p>I hope you found this helpful. Let me know what you think; I appreciate any and all feedback. And if you've got any cool privacy techniques up your sleeve, share the knowledge!</p>
	
<hr style="width:25%; margin-top: 2em"/>

<a name="footnote_1"></a><p>[1] A cryptographic hash is way of mapping any amount of plain text to a fixed-length "fingerprint" such that the same text always maps to the same hash, and given a hash, it is impossible to generate the text from which it was derived. Hashes are wonderful things with many uses. If you're a developer, and you didn't already know this, stop reading now and go <a href="http://en.wikipedia.org/wiki/Cryptographic_hash">here</a> or <a href="http://www-128.ibm.com/developerworks/java/library/s-hashing/index.html">here</a>, and learn how to generate a SHA1/2 hash in your programming language of choice. Come back when you're ready. I'll wait.</p>

<a name="footnote_2"></a><p>[2] You can throw in a <a href="http://en.wikipedia.org/wiki/Salt_%28cryptography%29">salt</a> too, to be safe; just make sure that you're not using the same hash that you're using for checking the user's password. You <em>are</em> smart enough not to <a href="http://blog.moertel.com/articles/2006/12/15/never-store-passwords-in-a-database">store passwords in plaintext in the database</a>, aren't you?</p>

<a name="footnote_3"></a><p>[3] Danger, Will Robinson! Keep reading.</p>
