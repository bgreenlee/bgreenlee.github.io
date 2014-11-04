--- 
title: "Swift Compiler Bugs"
date: 2014-11-04 05:27:31 -0800
layout: post
---

[Swift](https://developer.apple.com/swift/) is a great language, and far more enjoyable to program in than Objective-C, but it is still in its infancy, and bugs abound. One of the more frustrating is compiler errors that are flat-out wrong. Take a look at this (in Xcode 6.1):

![Could not find member](/images/could_not_find_member.png)

"Could not find member 'date'?" Trust me when I tell you that `date` is indeed a member of a `Todo` object. So what's going on? Let's tweak it a bit:

![Cannot invoke](/images/could_not_find_member_better.png)

Ah, there we go. Turns out you can't actually compare dates like that (hmm...I smell a follow-up post on writing extensions). Here's how you do it:

![Fixed](/images/could_not_find_member_fixed.png)

Here's another one. The following is essentially a no-op:

```swift
for i in 0..<0 {
    println(i)
}
```

That's just a for loop from zero up to, but not including, zero. And this code is fine, doing what it is supposed to do—nothing.

However, try making the ending value of the loop negative. This should be equivalent to the above:

```swift
for i in 0...(-1) {
    println(i)
}
```

The `...` operator means "up to and including". So this should also be a no-op (and it is in most programming languages). But:

![For Loop Boom](/images/for_loop_boom_1.png)

Even worse, it will actually crash on the first executable line of code:

![For Loop Boom 2](/images/for_loop_boom_2.png)

None of this should scare you away from using Swift. It's a fun language to work with, and ends up being much more concise than Objective-C. Just keep in mind that there are still bugs to be worked out, and if you're getting errors that just make no sense, there's a fair chance you've hit one.


