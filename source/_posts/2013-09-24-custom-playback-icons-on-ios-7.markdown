---
layout: post
title: "Custom Playback Icons on iOS 7"
date: 2013-09-24 10:20
comments: true
categories: iOS7
---

I've been having a HARD time getting remote control events for music apps to work properly on iOS 7.
It doesn't matter the app, Spotify, Downcast, even Music.app won't let me control the playback
via the media scrubber on the control center. 

Let me correct myself, it works for approximately an hour after doing a full reboot on the device.
After that time, all playback controls seem to stop functioning. I finally decided to 
submit a bug report to apple because of this (others have told me they are having the same 
issue, but I wanted to formalize it). In the process of preparing a bug report with device
logs and screen shots, I discovered something interesting!

## The Discovery

{% img /images/posts/custom-playback.png %}   
While this may look like your typical "I'm playing something" lock screen, take a closer look.
Instead of a back button, there is a star. This is because I'm playing via iTunes Radio. 
This star allows you to basically "favorite" a song that is playing and tells iTunes to 
play more like that song.   

While I'm no expert on MPMediaPlayback, I can't seem to recall having seen this ability before.
A quick dive into the most recent documentation makes no mention of the ability to customize it.
I also checked in the UIEvent Remote Control Event Types and there's nothing mentioned about
a custom function.  
``` objective-c
typedef enum {
   UIEventSubtypeNone                              = 0,
   
   UIEventSubtypeMotionShake                       = 1,
   
   UIEventSubtypeRemoteControlPlay                 = 100,
   UIEventSubtypeRemoteControlPause                = 101,
   UIEventSubtypeRemoteControlStop                 = 102,
   UIEventSubtypeRemoteControlTogglePlayPause      = 103,
   UIEventSubtypeRemoteControlNextTrack            = 104,
   UIEventSubtypeRemoteControlPreviousTrack        = 105,
   UIEventSubtypeRemoteControlBeginSeekingBackward = 106,
   UIEventSubtypeRemoteControlEndSeekingBackward   = 107,
   UIEventSubtypeRemoteControlBeginSeekingForward  = 108,
   UIEventSubtypeRemoteControlEndSeekingForward    = 109,
} UIEventSubtype;
```  

Could this be an upcoming feature added to SDK 7.1? If anyone has any more info about it, 
I'd love to hear!

