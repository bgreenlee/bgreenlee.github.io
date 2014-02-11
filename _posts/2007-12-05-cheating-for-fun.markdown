--- 
wordpress_id: 81
title: Cheating for Fun
date: 2007-12-05 09:16:58 +00:00
wordpress_url: http://blog.footle.org/2007/12/05/cheating-for-fun/
layout: post
---
I recently checked out the <a href="http://www.facebook.com/applications/Scramble/6494671374">Scramble</a> application on Facebook, which is a knock-off of <a href="http://en.wikipedia.org/wiki/Boggle">Boggle</a>. It's a fun diversion, even though I usually get my ass kicked. There's an annoying chat window there, too, and invariably someone will accuse someone else of cheating, at which point a number of people will pipe up with "how would you cheat?" 

This got me thinking...how *would* you cheat? You'd just have to write a Scramble puzzle solver that spit out all the words in on the board. That sounded like a fun problem. I hadn't done anything with <a href="http://en.wikipedia.org/wiki/Recursion">recursion</a> in a while, so I gave it a go. Below is what I came up with. You'll need a word list to run it. I originally used /usr/share/dict/words, but it was missing a lot of valid words (I think it only has base words, no plurals or tenses). So I stole the scrabble word list from the <a href="http://packages.debian.org/sarge/games/scrabble">Debian scrabble</a> package, which worked much better.

Anyway, here you go:

* [boggler.rb](https://gist.github.com/bgreenlee/8946388)
* [words.gz](/files/words.gz)

I did try this out a couple times on Scramble and won handily. There's really not much fun in that, though.

<b>Note:</b> This was really just an exercise in coming up with a fun algorithm, and is primarily intended for programmers. If you're just looking to cheat at Boggle/Scramble, there are plenty of <a href="http://www.l.google.com/search?q=cheat+boggle">web sites that will help you with that</a>, and with a lot less effort.
