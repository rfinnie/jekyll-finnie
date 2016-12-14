---
id: 1925
title: 2ping 1.2 released
date: 2011-12-24T01:32:50+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/?p=1925
permalink: /2011/12/24/2ping-1-2-released/
categories:
  - Uncategorized
tags:
  - 2ping
  - planet:canonical
---
[2ping 1.2](http://www.finnie.org/software/2ping/) has been released, adding ping-style mdev/ewma statistics:

  * Added exponentially-weighted moving average (ewma) and moving standard deviation (mdev) statistics to the summary display

2ping is a bi-directional ping utility. It uses 3-way pings (akin to TCP SYN, SYN/ACK, ACK) and after-the-fact state comparison between a 2ping listener and a 2ping client to determine which direction packet loss occurs.
