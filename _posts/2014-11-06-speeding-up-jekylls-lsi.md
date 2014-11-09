---
title: "Speeding up Jekyll's LSI"
date: 2014-11-06 08:51:59 -0800
layout: post
summary: Ruby is slow as hell.
tags: jekyll
---

Jekyll's Latent Semantic Indexing (LSI) indexes your posts to create the "Related Posts" section you see at the bottom of the individual post pages. However, if you have more than a handful of posts, it is very slow, and it seems to get exponentially slower as the number of posts increase. I ran it on this site, with ~140 posts, and I finally killed it after 3 hours.

There's a fix for this, though, buried in the README for [classifier-reborn](https://github.com/jekyll/classifier-reborn), the library used to do the post classification. Install [GNU GSL](http://www.gnu.org/software/gsl) and [rb-gsl](https://rubygems.org/gems/rb-gsl). On OS X with [homebrew](http://brew.sh/), it's as easy as:

```bash
brew install gsl
gem install rb-gsl
```

Indexing time for me went from, well, 3-∞ hours to less than 5 seconds.
