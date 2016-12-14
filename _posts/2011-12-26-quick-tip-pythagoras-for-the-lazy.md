---
date: 2011-12-26 19:30:46-08:00
layout: post
tags:
- planetcanonical
title: 'Quick tip: Pythagoras for the lazy'
wp_id: 1929
---
I occasionally plug this into [Wolfram Alpha](http://www.wolframalpha.com/):

> a^2+b^2=c^2, a/b=16/9, c=27

Click the "approximate forms" solution to get the width and height (_a_ and _b_) for a rectangle where you know the diagonal (_c_) and the ratio (16/9). _a_ or _b_ can be specified at the end instead of _c_ if you know the width or height.

I most often use this when I need to get the physical width and height of a monitor panel that I know the diagonal size of (since nearly all monitors are advertised by their diagonal panel size). With that information and the resolution, you can figure out the physical DPI of the monitor. (Not to be confused with the _effective DPI_ of the operating system, which is used for things like converting font [points](http://en.wikipedia.org/wiki/Point_%28typography%29) and [ems](http://en.wikipedia.org/wiki/Em_%28typography%29) to [pixels](http://en.wikipedia.org/wiki/Pixel), and is usually independent of the monitor's size and resolution: 96 DPI for Windows, 72 DPI for Mac OS, and 75 or 100 DPI for X11 historically, though many Linux distros are preset to 96 DPI today.)
