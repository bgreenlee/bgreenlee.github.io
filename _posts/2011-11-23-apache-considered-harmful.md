--- 
title: "Apache Considered Harmful"
date: 2011-11-23 11:15:52 -0800
layout: post
---
A [recent article by Mikeal Rogers ](http://www.futurealoof.com/posts/apache-considered-harmful.html) about the Apache Software Foundation's outmoded idea of open source contributions struck a chord with me. 

Some time ago I submitted a [pull request](https://github.com/cloudera/flume/pull/12) to Flume, making a very minor change to get something to compile again after code reorganization had broken it. About a week later, I got a comment from one of Cloudera's engineers saying that the patch looked good, but that since they were in the process of moving to Apache Incubator, could I follow some [extra steps](https://github.com/cloudera/flume/wiki/HowToContribute). The extra steps (create an account on Cloudera's JIRA issue tracker, create an issue for the bug, generate a patch for my change and attach it to the issue) weren't terribly onerous, but considering I had already moved on (and in fact had decided not to use Flume for my project), and had real work to get done, I put it on the back burner and eventually forgot all about it.

The genius of git and GitHub is how easy it is to contribute to projects. Fork, fix, submit. [I](https://github.com/defnull/bottle/commits/master?author=bgreenlee) [submit](https://github.com/AloneRoad/mogilelocal/commits/master?author=bgreenlee) [patches](https://github.com/github/github-services/commits/master?author=bgreenlee) [to](https://github.com/spagalloco/airbrake-api/commit/e9abeefb8b5cd573c6bd0902e4775a32cab2c8b8) [projects](https://github.com/boto/boto/commits/master?author=bgreenlee) [all](https://github.com/defunkt/gist/commits/master?author=bgreenlee) [the](https://github.com/collectiveidea/tinder/commits/master?author=bgreenlee) [damn](https://github.com/rails/rails/commits/master?author=bgreenlee) [time](https://github.com/datamapper/extlib/commits/master?author=bgreenlee) because barrier to contribution is so low. Like most developers, I've got a job that keeps me busy, and any roadblocks big enough to take me out of my flow are usually going to stop me.

Someone more patient than I submitted a ["proper" patch](https://issues.cloudera.org/browse/FLUME-669) for my Flume issue about a month later, so my conscience is clear. I hope, however, that the ASF eventually embraces the new open source reality. Their software will be better for it.




