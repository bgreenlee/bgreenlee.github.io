--- 
wordpress_id: 122
title: "Better Dictionary: Wordnik"
date: 2009-12-22 10:53:39 +00:00
wordpress_url: http://blog.footle.org/?p=122
layout: post
---
Ok, thanks to Google for putting their <a href="http://www.google.com/dictionary">dictionary</a> out there, which I wrote about <a href="http://blog.footle.org/2009/12/04/google-dictionary-bookmarklet/">a couple of posts ago</a>, but the quality of the results pales in comparison to <a href="http://www.wordnik.com">Wordnik</a>, which I've just discovered. It won't do language translation for you, but as far as the English language is concerned, this is pure word porn. So here's an updated bookmarklet for you:

<a href="javascript:(function(){var%20s;if(window.getSelection){s=window.getSelection();}else%20if(document.selection){s=document.selection.createRange();}window.open('http://www.wordnik.com/words/'+escape(s));}());">wordnik</a>

Just drag that to your bookmarks bar, select a word or phrase on the page, and click. Happy bonus: just clicking on the bookmarklet without highlighting a word first will give you a random word (thanks to Wordnik, not me). Cool!
