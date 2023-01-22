---
title: "The Case of the Absurdly Large Numbers"
date: 2023-01-21 18:10:08 -0800
layout: post
tags: debugging javascript browsers
---

One of the engineers on my team at Etsy, who is working on getting our clickstream events into [protobuf](https://developers.google.com/protocol-buffers), noticed that we were getting some values that were too large for protobuf's largest integer, uint64. E.g: 55340232221128660000.

This value was coming from our frontend performance metrics-logging code, and is supposed to represent the sum of the [encodedBodySizes](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceResourceTiming/encodedBodySize) of all the JavaScript or CSS assets on the page. But given that 55340232221128660000 is around 50,000 petabytes, it's probably not right. There had to be a bug somewhere.

We were only seeing this on a small percentage of visitors to the site—less than 0.1%. I looked at the web browsers logging these huge values and couldn't see any obvious pattern.

My first thought was that we had code somewhere that was concatenating numbers as strings. In JavaScript, if you start with a string and add numbers to it, you get concatenated numbers. E.g.:

```javascript
> "" + 123 + 456
< '123456'
```

But I didn't see anyway that could happen in our code.

Looking at the different values we were getting, I saw a lot of similar-but-not-quite equal values. For example:

```
18446744073709552000
18446744073709556000
18446744073709654000
36893488147419220000
36893488147419310000
55340232221128660000
55340232221128750000
55340232221128770000
73786976294838210000
92233720368547760000
```

The fact that they all end in zeros was curious, but I realized that was because they were overflowing [JavaScript's max safe integer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER).

I started googling some of the numbers and [got a hit](https://stackoverflow.com/questions/21568738/parseint18446744073709551616-returning-18446744073709552000-in-javascript). I thought it was very odd that one of our seemingly random numbers was showing up on a StackOverflow post. Then I googled "18446744073709551616". Any guesses what that is?

2<sup>64</sup>

36893488147419220000? Approximately 2 * 2<sup>64</sup>

55340232221128660000 =~ 3 * 2<sup>64</sup>

etc.

I realized what was probably happening was when we were adding up all the resources on the page, some of them were returning 2<sup>64</sup>-1 (the maximum 64-bit unsigned integer), but some were normal, so that explained the variation.

Now [encodedBodySize is an `unsigned long long`](https://www.w3.org/TR/resource-timing/#sec-performanceresourcetiming:~:text=readonly%20attribute%20unsigned%20long%20long%20%20encodedBodySize%3B), so my first thought was there is a browser bug that was casting a -1 into the unsigned long long, which would give us 2<sup>64</sup>-1.


I started digging through browser source code. I found that in WebKit, [the default value for `responseBodyBytesReceived`](https://github.com/WebKit/WebKit/blob/5f39acd5d511b9b48d7681413534d6d14f8117e5/Source/WebCore/platform/network/NetworkLoadMetrics.h#L109) is 2<sup>64</sup>-1, although [that value should be caught and 0 returned instead](https://github.com/WebKit/WebKit/blob/3a95196448a44d0b61cdd5e4825c22328cf90b26/Source/WebCore/page/PerformanceResourceTiming.cpp#L296-L297).


Then I went spelunking in the Chromium source, and found [this commit](https://github.com/chromium/chromium/commit/a378e342dd91b17f9d9fd9a95b382ac94da26875), from less than two weeks ago. (Although [the bug was reported back in May 2022](https://bugs.chromium.org/p/chromium/issues/detail?id=1324812).) Sure enough, it was a default value of -1 getting cast to an unsigned value. Boom! Mystery solved.

I pushed a fix to our JavaScript to catch these bogus values and hopefully our protos will live happily ever after. Gone are my dreams of finding and fixing a major browser bug, but I'm happy that I can move on with my life.