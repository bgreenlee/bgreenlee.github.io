--- 
wordpress_id: 111
title: Google Dictionary Bookmarklet
date: 2009-12-04 15:52:25 +00:00
wordpress_url: http://blog.footle.org/?p=111
layout: post
---
Google recently launched a decent <a href="http://www.google.com/dictionary">dictionary</a>, so for you logophiles, here's a bookmarklet that will bring up the definition of whatever text you have selected on the page: <a href="javascript:(function(){var%20s;if(window.getSelection){s=window.getSelection();}else%20if(document.selection){s=document.selection.createRange();}window.open('http://www.google.com/dictionary?aq=f&langpair=en|en&hl=en&q='+s);}());">gDefine</a>. Just drag it to your bookmarks toolbar, then double-click a word somewhere and click the button to get a definition.

It is set to use the English dictionary, but if you want to use it in a different language (or even translate between languages), just change the <code>langpair</code> parameter from <code>en|en</code> to, say,<code> fr|fr</code> for French, <code>de|de</code> for German, etc. (try a search on the Google Dictionary site in the language you want and look to see what the <code>langpair</code> parameter is in the url). Also, the <code>hl</code> parameter controls the language of the results page, so if you want your results to be in that language as well (as opposed to English), change that accordingly.

Here are a few to get you started:

<a href="javascript:(function(){var%20s;if(window.getSelection){s=window.getSelection();}else%20if(document.selection){s=document.selection.createRange();}window.open('http://www.google.com/dictionary?aq=f&langpair=fr|fr&hl=fr&q='+s);}());">gDefine (French)</a>
<a href="javascript:(function(){var%20s;if(window.getSelection){s=window.getSelection();}else%20if(document.selection){s=document.selection.createRange();}window.open('http://www.google.com/dictionary?aq=f&langpair=es|es&hl=es&q='+s);}());">gDefine (Spanish)</a>
<a href="javascript:(function(){var%20s;if(window.getSelection){s=window.getSelection();}else%20if(document.selection){s=document.selection.createRange();}window.open('http://www.google.com/dictionary?aq=f&langpair=de|de&hl=de&q='+s);}());">gDefine (German)</a>

And translations:

<a href="javascript:(function(){var%20s;if(window.getSelection){s=window.getSelection();}else%20if(document.selection){s=document.selection.createRange();}window.open('http://www.google.com/dictionary?aq=f&langpair=fr|en&hl=en&q='+s);}());">gTranslate (French>English)</a>
<a href="javascript:(function(){var%20s;if(window.getSelection){s=window.getSelection();}else%20if(document.selection){s=document.selection.createRange();}window.open('http://www.google.com/dictionary?aq=f&langpair=en|fr&hl=en&q='+s);}());">gTranslate (English>French)</a>

Enjoy!
