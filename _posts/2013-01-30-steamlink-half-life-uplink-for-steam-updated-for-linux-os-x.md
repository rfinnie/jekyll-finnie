---
author: Ryan Finnie
categories:
- Uncategorized
date: 2013-01-30 19:10:17
guid: http://www.finnie.org/2013/01/30/steamlink-half-life-uplink-for-steam-updated-for-linux-os-x/
id: 2218
layout: post
permalink: /2013/01/30/steamlink-half-life-uplink-for-steam-updated-for-linux-os-x/
tags:
- planet:canonical
title: 'SteamLink (Half-Life: Uplink for Steam) updated for Linux / OS X'
---
The original Half-Life and Counter-Strike games were [quietly released for Linux](http://www.geek.com/articles/games/valve-ports-half-life-to-linux-20130125/) [and Mac OS X](http://tech2.in.com/news/gaming/valve-quietly-ports-halflife-to-osx-and-linux/725422) last week, and as the maintainer of [SteamLink](http://www.halflifeuplink.com/steamlink/), a repackaging of [Half-Life: Uplink](http://www.halflifeuplink.com/) for [Steam](http://www.steampowered.com/), I went out to see if the mod's files could be installed on these platforms.

Turns out there is [a bug](https://github.com/ValveSoftware/steam-for-linux/issues/960#issuecomment-12883287) in Steam for these platforms, where it tries to launch the Windows version of Half-Life for GoldSrc mods from within Steam. However, Half-Life can be manually launched and pointed at the mod.

I have released [a new version of SteamLink](http://www.halflifeuplink.com/steamlink/) as a zip file. If you would like to run Half-Life: Uplink on Linux or OS X, simply download and extract the zip, and run the installer shell script. It will determine the Half-Life installation directory, install the mod, and give you a symlink to a script to launch it.
