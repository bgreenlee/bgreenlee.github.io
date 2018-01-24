---
title: "Rescuing Windows with Hammerspoon"
date: 2018-01-23 21:02:42 -0800
layout: post
tags: osx automation lua
---

I recently got a new monitor and it came with a problem. For some reason when I attached or detached my laptop, I would end up with many of my windows off the edge of the screen, and I'd have to drag them back over one by one. It was annoying enough that I decided I'd write an app to "rescue" them.

I figured it wouldn't be too hard. Get a list of active windows, see which are off-screen, and move them over. But it was hard. My code only half-worked. And for some reason I couldn't get Chrome windows to move.

I started looking around and found [Swindler](https://github.com/tmandry/Swindler). It seemed to be just what I needed and confirmed that I wasn't completely incompetent:

>Writing window managers for macOS is hard. There are a lot of systemic challenges, including limited and poorly-documented APIs. All window managers on macOS must use the C-based accessibility APIs, which are difficult to use and are surprisingly buggy themselves.

But then I saw [Hammerspoon](http://www.hammerspoon.org/) listed at the bottom of the Swindler README, under Related Projects. Intrigued, I checked it out. It was just what I needed.

Hammerspoon lets you write Lua scripts to interact with the MacOS system, and it's pretty powerful. The [Getting Started Guide](http://www.hammerspoon.org/go/) gives a good sense of some of the things you can do with it.

I'd never written Lua before, but with plenty of examples to crib from and good [API documentation](http://www.hammerspoon.org/docs/index.html), it didn't take me long to come up with a little script that did just what I needed:

{% gist 77f1fbeb673eb674ed6065b189dec4d3 %}

I have that file saved at `~/.hammerspoon/rescuewindows.lua`, and in `~/.hammerspoon/init.lua` I have:

```
local rescueWindows = require "rescuewindows"
hs.hotkey.bind({"cmd", "alt", "ctrl"}, "R", rescueWindows)
```

So when it hit ⌃⌥⌘R, all my off-screen windows slide over to my main screen. Nice!
