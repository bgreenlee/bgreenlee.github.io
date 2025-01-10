---
title: "Screensaver Spelunking"
date: 2024-11-07 09:15:16 -0800
layout: post
tags: macos
---

Macos Sonoma brought to the Mac the [amazing aerial screensavers/wallpapers](https://bzamayo.com/watch-all-the-apple-tv-aerial-video-screensavers#9c6b969b62012359e5a4ead2ba3889e8) that can be found on the AppleTV. (I say screensavers/wallpapers because you can choose to have your wallpaper be a frame of whatever your screeensaver is.)

One thing missing, though, are the descriptions of what is being displayed. On the AppleTV you can view the description by pressing up on the remote. Surely that information is available, though, as if you go into Settings -> Screen Saver on your Mac, you'll see descriptions:

![Screensaver Descriptions](/public/images/screensavers/screensaver-settings.png)

I figured it couldn't be that hard to (a) find out what the current screensaver is, (b) look up the description somewhere, and (c) find easy way to show that information. Turns out, I was wrong.

In case you're here looking for answers to the same problem I was trying to solve, let me say up front that I have not figured this out. I'm writing this both as a way of putting my notes somewhere more permanent than the text file on my Desktop, and with the hope that someone else might have some insight into solving it.

## The Screensaver Database

It did not take long to find the database of screensavers/wallpapers. There's a sqlite database at `/Library/Application\ Support/com.apple.idleassetsd/Aerial.sqlite`

