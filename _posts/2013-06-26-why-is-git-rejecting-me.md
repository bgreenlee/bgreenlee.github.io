--- 
title: "Why is Git Rejecting Me?"
date: 2013-06-26 21:21:21 -0700
layout: post
tags: git
summary: How to push when git doesn't let you.
---
Every Git user, from novice to expert, gets this error message at some point, maybe even daily:

{% highlight bash %}
$ git push origin master
To git@github.com:bgreenlee/myrepo.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git@github.com:bgreenlee/myrepo.git'
hint: Updates were rejected because a pushed branch tip is behind its remote
hint: counterpart. Check out this branch and merge the remote changes
hint: (e.g. 'git pull') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
{% endhighlight %}

There are a couple ways this can happen, but it all boils down to this: **the remote repository you're pushing to has one or more commits that you don't have in your local repository**. Don't worry, though, the fix is easy, although how to go about it depends on what you're trying to do.

## Refresher: Rebase vs. Merge

Before we dive in, a quick refresher on rebase vs. merge. These are two general strategies for combining two branches.

Consider the following case:

    master: A -> B -> C
    my-fix:       \-> D

Here, you created a branch named “my-fix” from master while it was at commit B. While you were working on your fix, someone else pushed commit C to master. You finish your fix and commit it to the my-fix branch. That's commit D above.

Now you're ready to merge your my-fix branch back into master. There are two ways to go about this.
 
### Merge

You can do:

{% highlight bash %}
git checkout master
git merge my-fix
{% endhighlight %}

This will resolve the differences between the two branches by doing a three-way merge using commits C and D and their common ancestor, B. It does this by creating an additional merge commit, which we’ll call M:

    master: A -> B -> C -> M(B,C,D)

### Rebase

You can also do:

{% highlight bash %}
git checkout my-fix
git rebase master
git checkout master
git merge my-fix
{% endhighlight %}

The first checkout is just to make sure you're starting on the right branch. You may already be on the my-fix branch, in which case you can skip it.  What git rebase master does is “rewind” your branch to the point at which it diverged from master, B, then apply all the commits from master made after that point (C), then “replay” your commits to my-fix (D). What you get is:

    my-fix: A -> B -> C -> D’

Note that I used D’ above. This is because while the content of the commit is the same as your original commit D, it's technically a different commit, with a different SHA. This will be important later.

The last two commands switch back to master and then merge my-fix with master. **Since at this point my-fix has everything that master has, plus one extra commit, the merge is just a “fast-forward”, meaning that all git has to do is move the HEAD pointer to D’—no merge commit is required.**

Now, merging vs. rebasing is somewhat of a religious issue, although in my experience, most organizations prefer rebasing, as it keeps the commit history from getting cluttered with merge commits. I am going to go on the assumption that rebasing is the preferred method.

Now, let's get back to fixing our problem.

## Scenario 1: Pull, then Push

This is the most common scenario, and simplest fix. Say you've made some changes in the master branch of your local repository, then go to push them to the master branch of the remote repository. If your push is rejected, what has most likey happened is that someone else pushed some changes to the remote master while you were making your changes, and you need to pull them down to your repo before you can push your changes up. So do a 'git pull --rebase', then push again.

## Scenario 2: Force Push

Let's say you've been working on your own branch, and suddenly git is rejecting you. Now you know that no one else has been pushing commits to that branch, so what happened?

Most likely, you updated your local branch by rebasing against master. Again, this “rewinds” the commits in your branch to the point where they diverged from master, then pulls in the commits from master that you don’t have, then “replays” the commits in your branch that weren’t in master **as new commits**. So when you go to push those to your remote branch, even though it may look like the commits are the same, the SHAs are different, so git will refuse to update the remote branch.

One way around this is to force push:

{% highlight bash %}
git push -f origin my-fix
{% endhighlight %}
    
This tells git that you don’t care about what’s on the remote branch, just update it with what you have locally. **This can be dangerous. Don’t do it unless you know what you’re doing.** You would only ever want to do this if you are sure that you are the only one working on that branch, and that you have all the commits locally that are on the remote branch (i.e. you didn’t push changes from another machine). If that’s not the case, and, say, a coworker had pushed some of her own changes to your branch, you will overwrite those changes. Now, chances are they will be recoverable (she probably still has them in her local repository), but digging yourself out will be painful.

If you are in a situation where you've rebased your branch and meanwhile your colleagues have pushed changes to the remote branch, one way to get back on track is to  cherry-pick their commits into your branch, and then force-push your branch (after telling your colleagues to hold off on pushing):

{% highlight bash %}
git fetch  # to make sure your repo knows about their commits
git log origin/my-fix  # to see what their commits are, and copy the shas
git cherry-pick <sha1>
git cherry-pick <sha2>
git push -f origin my-fix
{% endhighlight %}

Your colleagues will then have to delete their local branch and pull it down again.

I’m sure I’ll get some hate mail for suggesting force push as a viable option, so I’ll reiterate: don’t do this unless you know what you’re doing. Even if you do know what you’re doing, you’re playing with fire, as a developer at my company learned when he accidentally force-pushed to `origin master`, which brought most of the engineering team to a halt while we figured out what got blown away and reconstructed master. (To git's credit, it is hard to do real irreversible damage; between the [reflog](http://gitready.com/intermediate/2009/02/09/reflog-your-safety-net.html) and commits stored in your colleagues' repos, you can almost always recover. It just may take some time to sort things out.)

I should also note that it is possible to disable force-pushing on a per-repository basis (unfortunately not per-branch, AFAIK, but there are hoops you can jump through).

