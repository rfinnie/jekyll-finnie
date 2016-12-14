---
id: 1785
title: 2ping 1.1 released
date: 2011-04-05T14:05:07+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/?p=1785
permalink: /2011/04/05/2ping-1-1-released/
categories:
  - Uncategorized
tags:
  - 2ping
  - planet:canonical
---
[2ping 1.1](http://www.finnie.org/software/2ping/) has been released, with a few small changes:

  * Host processing delays sent by the peer are no longer considered when calculating RTT
  * Changed ID expiration (for which no courtesty was received) time from 10 minutes to 2 minutes
  * Manpage fix: correct UDP port number listed
  * Added an RPM spec file

2ping is a bi-directional ping utility. It uses 3-way pings (akin to TCP SYN, SYN/ACK, ACK) and after-the-fact state comparison between a 2ping listener and a 2ping client to determine which direction packet loss occurs.
