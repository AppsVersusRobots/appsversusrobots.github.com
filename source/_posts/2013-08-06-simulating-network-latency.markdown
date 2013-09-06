---
layout: post
title: "Simulating Network Latency"
date: 2013-08-06 22:51
comments: true
categories: snippets
---

Whenever I'm in need of a good old fashioned loading screen, and I'm not in the mood for
really doing network communications, I will use this handy GCD trick.

``` objective-c
NSInteger delayTime = 2;
dispatch_after(dispatch_time(DISPATCH_TIME_NOW, delayTime * NSEC_PER_SEC), dispatch_get_current_queue(), ^{        
     [self updateViewWithData];
});
```

It isn't the prettiest solution, but sometimes you just want to get the feeling the user will
be feeling while on a crappy network somewhere.