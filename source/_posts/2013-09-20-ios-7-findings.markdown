---
layout: post
title: "iOS 7 Findings"
date: 2013-09-20 10:34
comments: true
categories: [xcode, iOS7]
---
[1]: http://www.macrumors.com/2013/06/10/apple-releases-ios-7-beta-1-to-developers/
[2]: http://www.geekbinge.com/wp-content/uploads/2013/07/Andy-Dwyer-Dancing-Background.gif
[3]: https://github.com/enormego/EGOTableViewPullRefresh

## Diving into iOS 7

While XCode 5 and iOS 7 beta have been out [for a while][1], I have been so busy
that I haven't had the chance of diving into it. That is, until this week. I've
[LITERALLY][2] been compiling for iOS 7 SDK for 3 days, so if any of my findings are
incorrect, feel free to let me know!

### Pull To Refresh
I've been using [EGOTableViewPullRefresh][3] probably WAY past its prime and now it is showing.
Here is the pre-iOS7 code that I was using:
``` objective-c
- (void)scrollViewDidScroll:(UIScrollView *)scrollView{	
	
	if (scrollView.isDragging) {
		if (refreshHeaderView.state == EGOOPullRefreshPulling && scrollView.contentOffset.y > -65.0f && scrollView.contentOffset.y < 0.0f && !_reloading) {
			[refreshHeaderView setState:EGOOPullRefreshNormal];
		} else if (refreshHeaderView.state == EGOOPullRefreshNormal && scrollView.contentOffset.y < -65.0f && !_reloading) {
			[refreshHeaderView setState:EGOOPullRefreshPulling];
		}
	}
}

- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate{
	
	if (scrollView.contentOffset.y <= - 65.0f && !_reloading) {
        _reloading = YES;
        //[self reloadTableViewDataSource];
        [self reloadTableViewDataSource];
        [refreshHeaderView setState:EGOOPullRefreshLoading];
        [UIView beginAnimations:nil context:NULL];
        [UIView setAnimationDuration:0.2];
        self.tableView.contentInset = UIEdgeInsetsMake(60.0f, 0.0f, 0.0f, 0.0f);
        [UIView commitAnimations];
	}
}

- (void)dataSourceDidFinishLoadingNewData{
 	
	_reloading = NO;
	
	[UIView beginAnimations:nil context:NULL];
	[UIView setAnimationDuration:.3];
	[self.tableView setContentInset:UIEdgeInsetsMake(0.0f, 0.0f, 0.0f, 0.0f)];
	[UIView commitAnimations];
	
	[refreshHeaderView setState:EGOOPullRefreshNormal];
	[refreshHeaderView setCurrentDate];  
 }
```
All of those contentInsets tend to break things quite nastily on iOS7. I've now switched to a UIRefreshControl for 
my table views and all is well in the world
``` objective-c
-(void)viewDidLoad
{
	self.refreshControl = [[UIRefreshControl alloc]init];
	[self.refreshControl addTarget:self action:@selector(reloadData) forControlEvents:UIControlEventValueChanged];
}
-(void)reloadData
{
	[self.refreshControl beginRefreshing];
	//Do the reload
	[self.refreshControl endRefreshing];
}
```
As you can see, all of the other cruft washes away and gets you my favorite kinds of commits "6 lines added, 50 lines removed"!

### Table Cell Backgrounds
I rely a lot on my table backgrounds to provide my color for my table views. I usually don't have much design
in each table cell. So, you can imagine my surprise when after compiling for iOS7, I see something like this:  
{% img /images/posts/iOS7-white.png 320 568 %}   
AHHH! So much white!!! Luckily, I have most of my custom table cells done as classes, so in the initialization of the class,
I simply have to:
```
self.backgroundColor = [UIColor clearColor];
```

### Swipe Navigation
I actually really like the "swipe to go back" navigation methods in iOS7, but for those who don't or if you find a place where
it might not be proper, you can disable it with:
```
self.navigationController.interactivePopGestureRecognizer.enabled = NO;
```

### Navigation Bar Coloring
There are now two different nav bar tinting properties:

   *   tintColor = color of the back button, button titles and images
   *   barTintColor = changes the color of the bar itself
   

### That's All, For Now
I'm sure I'll have plenty more to fill in on, but these were the first of my iOS 7 findings.
