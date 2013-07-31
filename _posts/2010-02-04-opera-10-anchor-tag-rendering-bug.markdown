--- 
wordpress_id: 125
title: Opera 10 Anchor Tag Rendering Bug
date: 2010-02-04 22:59:05 +00:00
wordpress_url: http://blog.footle.org/?p=125
layout: post
---
For some time now, <a href="http://wesabe.com">Wesabe</a>'s transaction edit dialog has been broken in <a href="http://www.opera.com/">Opera</a> (10.10 as of this writing). Since it was fine in all other major browsers, and none of us were Opera users, fixing it was never high priority. I thought I'd take a look at it again tonight, though, and just when I was about to give up and go to bed, I managed to track it down.

If you have an anchor (<code>&lt;a&gt;</code>) tag that contains <a href="http://htmlhelp.com/reference/html40/block.html">block-level elements</a> (which is <a href="http://www.w3.org/TR/html401/struct/links.html#edef-A">technically not allowed</a>, but all other browsers seem to be ok with it), such as <code>&lt;p&gt;</code> or <code>&lt;div&gt;</code>, the anchor will close itself before the elements it is supposed to contain. E.g.:
<code>
&lt;a&gt;
  &lt;p&gt;hello&lt;/p&gt;
&lt;/a&gt;
</code>
Will render as:
<code>
&lt;a&gt; &lt;/a&gt;
&lt;p&gt;hello&lt;/p&gt;
</code>

If you give the anchor an href, though, even if it is just a blank string (<code>&lt;a href=""&gt;...</code>), it renders fine.

Here's a page that demonstrates the bug: <a href="http://footle.org/misc/opera-bug.html">http://footle.org/misc/opera-bug.html</a>.

Anyway, I filed a bug with the Opera folks. Time for bed.
