--- 
title: "Code Signing Will Kill Me Yet"
date: 2016-03-31 15:25:19 -0700
layout: post
---
This is one of those I-spent-long-enough-stumped-on-this-issue-I-should-write-it-up-for-future-generations posts.

I wrote a little app at work (which I'll talk about in a future post), and I was having a strange code-signing issue. The app would sign just fine:

{% highlight bash %}
$ spctl --assess -v Orwell.app
Orwell.app: accepted
source=Developer ID
{% endhighlight %}

But then if I zipped it up and sent it to someone, when they unzipped it, OS X would tell them that it was damaged and should be thrown away. Sure enough, if I zip and unzip the app, and verify the codesigning again, I get:

{% highlight bash %}
$ spctl --assess -v Orwell.app
Orwell.app: a sealed resource is missing or invalid
{% endhighlight %}

After much hair-pulling, I found [Technical Note TN2318: Troubleshooting Failed Signature Verification](https://developer.apple.com/library/ios/technotes/tn2318/_index.html), which says:

> The file prefixed with ".\_" is problematic and a was the result of copying certain Mac OS X files to a non-HFS+ formatted disk. These files are referred to as Dot files, Apple Double files, or resource forks. They are invisible to Finder but can be removed using the dot\_clean utility.

Sure enough:

{% highlight bash %}
$ find Orwell.app -name ".\_*"
Orwell.app/Contents/Resources/app/node_modules/applescript/.\_package.json
Orwell.app/Contents/Resources/app/node_modules/applescript/lib/.\_applescript.js
Orwell.app/Contents/Resources/app/node_modules/applescript/samples/.\_execString.js
$ dot_clean build/Orwell-darwin-x64/Orwell.app
$ find build/Orwell-darwin-x64/Orwell.app -name ".\_*"
{% endhighlight %}

Running `dot_clean` before I signed the app fixed the issue.

(Orwell is an [Electron](http://electron.atom.io/) app, hence the `node_modules` dir, in case you were wondering.)

