--- 
wordpress_id: 120
title: Chrome Frame's Shifty User-Agent
date: 2009-12-15 14:49:48 +00:00
wordpress_url: http://blog.footle.org/?p=120
layout: post
---
One of the security measures we have in place at Wesabe is to invalidate a session and log the user out if their User-Agent or IP address subnet changes between requests. However, we recently had a number of complaints that users were getting logged out prematurely. Looking at one user's session in the logs, I noticed that their user agent string had "chromeframe/4.0" appended for some requests, but not others. It turns out that Google's <a href="http://code.google.com/chrome/chromeframe/">Chrome Frame</a> only <a href="http://groups.google.com/group/google-chrome-frame/browse_thread/thread/274e4c36aebbc02b/">modifies the user agent for top-level requests, and not for subsequent requests sent as that page loads</a>:

<blockquote>It was a compromise to keep code complexity down. In order to tag every request from IE...we have to have hooks in place at many more places (not to mention supporting the different things IE6, 7 and 8 do).  So, as a compromise we decided to keep it down to only the single hook that allows us to tag top level requests.</blockquote>

This fix on our side was to just strip the chromeframe identifier from the user agent string, although it makes me wonder what other browser extensions cause similar issues and whether invalidating a session when the user agent changes is even a worthwhile security measure.
