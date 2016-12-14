---
author: Ryan Finnie
categories:
- Uncategorized
date: 2011-04-20 12:51:07
guid: http://www.finnie.org/2011/04/20/contribution-intoxication/
id: 1797
layout: post
permalink: /2011/04/20/contribution-intoxication/
tags:
- planet:canonical
title: Contribution intoxication
---
I am not a programmer by trade. I'm a system administrator, which involves knowing _just enough_ about ten thousand different technologies to get by. A literal Jack of all trades, master of "none". (Well, few; of course I have specialties.)

That is not to say I don't know programming languages. As a quick list, I'm competent with Perl, PHP, Python, Javascript and Bourne shell, and know enough about C and Java to be able to add code here and there, but not be able to write a program from scratch. For the most part I can code well, but wouldn't want to do it for a living. (Which I tried back in 2000. Hated it, looking back.)

Now this post isn't all ego stroking (only about 90%). Being active in Free software, I often contribute patches to software projects. Usually one-line patches here and there, which is useful but not necessarily exciting. However, there are the occasional times where submitting a relatively small patch makes a huge improvement, and those are the sorts of contributions that really stick with me. Submitting these types of patches (and getting positive feedback from the developers or other users) honestly has a euphoria effect that lasts a long time.

A few examples:

* Last weekend I submitted a patch for [Minecraft Overviewer](https://github.com/brownan/Minecraft-Overviewer), which is a map renderer that makes isometric tiles out of Minecraft worlds, and wraps them in a [Google Maps interface](http://mc.colobox.com/map/?x=266&y=70&z=1041&zoom=-1). The problem is Minecraft has its own coordinates system that many users rely upon (X/Y/Z, where oddly Y is altitude), whereas Gmaps uses latitude/longitude coordinates. Overviewer shoehorned that into the Gmaps style by defining the world as a latitude and longitude between -1.0 and 1.0. [I wrote some code](https://github.com/brownan/Minecraft-Overviewer/pull/335) that, while internally everything still uses Gmaps-style lat/lng, the user interface shows/accepts Minecraft-style X/Y/Z coordinates everywhere.

* A few Debian versions ago, they switched the installation CD from a command line-based bootloader (straight syslinux) to a menu (syslinux menu.c32). However, menu.c32 didn't support conditional menu item selection based on 32/64 bit processors, which was [lamented in the prerelease post](http://lists.debian.org/debian-devel-announce/2008/06/msg00002.html). I saw this, thought "I know syslinux pretty well, that would probably be about a 10 minute patch!" Of course, 10 minutes turned into a few hours, but it was still relatively easy. [I submitted a bug report](http://bugs.debian.org/485656) with the patch, it was accepted into the syslinux package within an hour, and pulled into debian-installer an hour after that, ready for the night's daily builds. To date, I don't think I've ever seen Debian move that fast.

* The default functionality of Spamassassin is, when a spam is found, to wrap the original message in an RFC822 MIME container, add the spam report, and let it continue on. This is so the original message is perfectly preserved and can be extracted by many mail readers in case of false positives, submitting to an RBL, etc. That functionality was mine. In fact, I'm toward the top of the [CREDITS file](http://svn.apache.org/repos/asf/spamassassin/trunk/CREDITS) under "Major contributions", and tend to get the occasional mail from people who say "can you help me install?" or "how dare you block my perfectly legit penis enlargement mail!" by people who randomly mail the addresses they find in that file.

* And of course there's [Finnix](http://www.finnix.org/), which, while not a small patch contribution, produces the same effect. I'm always happy when I hear someone who uses it to get out of a tight situation, or lets them do their job easier.
