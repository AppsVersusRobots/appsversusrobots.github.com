---
layout: post
title: "Unwinding Segues: Wat?"
date: 2014-05-19 20:29:32 -0400
comments: true
categories:
---

[1]: http://www.meetup.com/Orlando-iOS-Developer-Group/events/175805382/
[2]: http://twitter.com/codefortravel
[3]: https://github.com/adamweeks/UnwindingSegue
[4]: http://twitter.com/adamweeks

I recently gave a talk at the ["Orlando iOS Meetup"][1] group about the
magic that is "Unwinding Segues". When I asked the group's leader, [Andrew][2]
if I could do a talk on them, he seemed as confused about them as I was
just a few weeks prior. Here's what I discussed:

>"One dog goes one way, the other dog goes the other way"
>-Goodfellas

For those familiar with storyboard segues, unwinding segues are the "other way"
that you can go in your app. Segues allow us to navigate and animate between
two view controllers in a storyboard. Before segues, we would have to write
code such as:
``` objective-c
-(IBAction)goToNextScreen:(id)sender
{
    NextViewController *nextVC = [[NextViewController alloc] init];
    [self.navigationController pushViewController:nextVC animated:YES];
}
```
Now, it is as easy as dragging a button to the view controller in the storyboard!
But what if we want to get back to where we came from? Sure, we could just put in
a quick 'pop' from the NextViewController, but Apple suggests against that
and it is just bad code. Also, what if you have data in NextViewController
that you want to pass back to our main view controller? I'm going to go over
three valid options you have for navigating back and passing data: Delegates,
Blocks and the new Unwinding Segues.

#Options:
###Background
In the following example code, we will use **MainViewController** and **NextViewController** to
represent the first and second view controllers. We'll
simply be passing a string from NextViewController back to MainViewController
and returning.

##Delegates
Before iOS 4, there really wasn't any easy way (outside of NSNotifications,
  but I said 'easy') to pass data between view controllers outside of delegate
  methods. Delegate methods allow a view controller to say "I respond to this
  command, so you can call that command on me".

  In NextViewController, we'll define a delegate protocol:
```objective-c
@protocol NextDelegate <NSObject>
@required
- (void)nextViewController:(NextViewController *)controller readyWithString:(NSString *)string;
@end
```
Then, you implement it in the MainViewController
```objective-c
- (void)nextViewController:(NextViewController *)controller readyWithString:(NSString *)string
{
  self.string = string;
  [self.navigationController popViewControllerAnimated:YES];
}
```
Call this from NextViewController when you are done with it
```objective-c
[self.nextDelegate nextViewController:self readyWithString:@"ready"];
```
This will send "ready" to MainViewController and MainViewController will pop the NextViewController off of
the navigation stack. There is a little more setup to get this fully working,
but we will skip it here. The code can be found in the sample project.

If we wanted to change the way views were pushed and popped to, say, presented
instead, the code would need to be changed on both the calling of NextViewController and the
delegate method that returns back to MainViewController. Imagine how complicated it would get
if you needed to get back two or three screens. We would need to make sure to
capture how we got there to properly get back.

##Blocks
Introduced in iOS 4, blocks allowed us pass around code as variables and
execute it when we saw fit. This makes for some pretty portable code,
unfortunately, we still have the issue of keeping track of how we got there.
(More on that later.)

For now, let's pass some data via blocks instead of defining delegates. We'll
make a property on NextViewController that is a block of string processing code:
```objective-c
@property (copy, nonatomic) void (^stringReadyBlock)(NSString *string);
```

When calling NextViewController from MainViewController, we'll define what we want to do with the string
```objective-c
__weak __typeof__(self) weakSelf = self;
NextViewController *nextVC = [[NextViewController alloc] init];
[nextVC setStringReadyBlock:^(NSString *string){
  weakSelf.string = string;
  [weakSelf.navigationController popViewControllerAnimated:YES];
}];
[self.navigationController pushViewController:nextVC animated:YES];
```
Notice that this is essentially the same code for what we did in the delegate
implementation up above. (We are using weakSelf to keep any memory issues at bay.)
But instead of having to define protocols and delegates,
we simply pass a block of code to a property. We'll execute this code in NextViewController with:
```objective-c
self.stringReadyBlock(@"ready");
```
Personally, this is how I handle all non-storyboard view navigation because
I just can't get enough of blocks.

##Unwinding Segues
Storyboards got a lot of attention when iOS 6 came out, but unwinding segues
seemed to fly under the radar. Marking a view controller as 'unwindable' is as
simple as implementing a method that matches a syntax. This code will be
in our MainViewController, since that is where we want to **UNWIND TO**. This was probably
the most confusing part for me to grasp, I always wanted to put this code in
the last view controller, not the first.
```objective-c
-(IBAction)unwindWithString:(UIStoryboardSegue *)segue
{
  NextViewController *nextVC = (NextViewController *)segue.sourceViewController;
  self.string = nextVC.string;
}
```
In this example, we've moved our string to a property on NextViewController. Now we just need
to tell the storyboard that we're ready to unwind. If you've been using
storyboards, you may have noticed a small green icon at the bottom of your
view controllers that resembles the 'sharing' icon in iOS. This is the view
controller's "exit". We will link up a button to our view controller's exit
by control-dragging from the button to the exit. Storyboard will look down the
presentation chain for the IBAction structure we defined above and let us
link to that segue.
{% img /images/posts/unwind.gif %}

Here's the great thing about unwinding segues, no matter how many view controllers
you put in between MainViewController and NextViewController, regardless of how you push and present, the
unwind will always take you right back to the beginning. There's no need to
keep track of how you got there, because storyboard already knows.

##Sample Project
These were just some sample code extracted from the project. You'll find the
full example [**on github**][3].

###Thanks!
Hope you enjoyed this dive into unwinding segues. If you have any questions,
concerns, comments, please contact me on twitter [@adamweeks][4].
