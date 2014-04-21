---
layout: post
title: "A Lesson in MVP: Gravity Pilot"
date: 2014-04-21 12:24:18 -0400
comments: true
categories:
---

[1]: https://developer.apple.com/library/ios/samplecode/DynamicsCatalog/Introduction/Intro.html
[2]:https://developer.apple.com/library/ios/documentation/GraphicsAnimation/Conceptual/SpriteKit_PG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013043-CH1-SW1
[3]:http://twitter.com/mhmazur
[4]:http://bit.ly/gravitypilot

>"For the things we have to learn before we can do them, we learn by doing them"
>
>-Aristotle

Following this quote is usually how I find myself learning new technologies.
When Apple announced the [UIKit Dynamics][1] at WWDC 2013, I didn't really have a use for it in the apps that I was working on, but was very intrigued.

Fast forward to February, I'm using the Starbucks app, watching the stars fall into a glass, when I decide to try to use UIKit Dynamics to emulate it for the water logger feature of Calorie Count. So I get to work learning UIKD.

With all the dynamics implemented, I have water bottles dropping into a giant cup, interacting with it and each other. It behaves very nicely and looks great. The next feature is to "remove" a water bottle from the glass. First, I just remove the last bottle added from the view, but since this is a learning experience, I decide that is too basic. Instead, I change the gravity affected on the last bottle to point upwards instead of downwards. This gives the bottle a nice floating away effect. During my testing, I find myself swapping the gravity on the bottle back and forth trying to keep the bottle hovering and bouncing up and down on the screen. Then it hit me, "This would make a fun game!"

{% img /images/posts/gravity-pilot1.png %}

So my goal with this game is to turn the gravity switching into a points system. You get points for touching the scoring areas at the top and bottom of the screen, without touching the edge of the screen which "kills" your player.

Quickly after starting development of this game in UIKit Dynamics, I realize, it isn't going to do what I want it to do. I take this opportunity to do another "learning lesson" and build the same game in [Sprite Kit][2]. In one night, I have a playable build that I send out to my friends. I also have a laundry list of features I want to implement within the game (Game Center, Leaderboards, Music, Graphics, etc.)! In walks [Matt Mazur][3] with a simple idea, release it now.

"But, I don't have any cool features." "No one will like it without all the extra stuff." "I've gotta have X feature, otherwise it will fail" -ME

"Release it. Now." -[Mazur][3]

So I suck up my pride and release a 1.0, devoid of anything but basic gameplay. Definitely a "MVP" (Minimum Viable Product) app. You know what? People are actually liking it! I'm getting feature requests that I wouldn't have thought of. I'm NOT getting feature requests for items that I thought I would.

Version 1.1 is pending review from Apple with some of those features, but I've got a nice core audience already hooked on the game that I wouldn't have had I not just "released it".

[Download Gravity Pilot on the App Store for free now!][4]
