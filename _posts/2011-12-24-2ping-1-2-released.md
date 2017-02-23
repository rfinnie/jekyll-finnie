---
date: 2011-12-24 01:32:50-08:00
layout: post
tags:
- 2ping
- planetcanonical
title: 2ping 1.2 released
wp_id: 1925
---
[2ping 1.2](https://www.finnie.org/software/2ping/) has been released, adding ping-style mdev/ewma statistics:

  * Added exponentially-weighted moving average (ewma) and moving standard deviation (mdev) statistics to the summary display

2ping is a bi-directional ping utility. It uses 3-way pings (akin to TCP SYN, SYN/ACK, ACK) and after-the-fact state comparison between a 2ping listener and a 2ping client to determine which direction packet loss occurs.
