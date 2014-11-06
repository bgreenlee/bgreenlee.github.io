--- 
wordpress_id: 33
title: "CSS Layouts: I Give Up"
date: 2005-09-27 16:57:00 +00:00
wordpress_url: http://footle.org/blog/?p=33
layout: post
---
<p><img src="/public/images/thescream_crop.jpg" width="165" height="202" alt="The Scream" style="float: left; padding: 0px 10px 10px 0px;"/>We're in the middle of a fairly major site redesign at <a href="http://triporama.com">Triporama</a>, and I was doing my best to have the layout be as purely CSS as possible, replacing the previous table-heavy layout. It's turned out to be a major headache, and I'm taking a step back now and reimplementing parts of the layout with tables.</p>

<p>One problem with CSS today is that every different browser has different levels of support for the different versions of CSS, whereas tables have been around for so long that most browsers in use today handle them fairly similarly. The really big problem with CSS, though, is IE. Internet Explorer is a <a href="http://www.positioniseverything.net/explorer.html">CSS bug fest</a>. It would be great to say "fuck IE," (actually, I say that almost every day) but it is unfortunately still the most widely used browser.</p>

<p>Yes, I know--tables aren't meant to be used for layout; they're not "semantic", they're slower (not sure about that one--maybe if you have many levels of nested tables). And yes, clever designers have come up with all sorts of tricks to get CSS layouts to work across different browsers. But you end up spending hours or days getting your layout to work on various browsers and what you have in the end is a confusing morass of fragile code. It's just not worth it. I'll wait for IE 7 plus a few years (although my parents will probably still be using AOL 8.0 on a dialup line) before attempting a pure-CSS site again.</p>

<p>So, to summarize my new web design philosophy: use CSS for various individual elements/components on the site, but if you're doing a multi-column layout, use a table for the main layout (actually, I still have my header and footer positioned with CSS, as I spent far too long figuring out how to get the footer to stay glued to the bottom of the page if you have less than a page worth of content, but to flow after the main content if you have more...and the solution I found seems to work on all modern browsers). The layout for the whole site is in a single file, so making site-wide changes (another argument for using CSS) is still easy.</p>
