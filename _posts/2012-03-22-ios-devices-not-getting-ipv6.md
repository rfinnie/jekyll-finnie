---
author: Ryan Finnie
date: 2012-03-22 21:17:54
guid: http://www.finnie.org/2012/03/22/ios-devices-not-getting-ipv6/
id: 2045
layout: post
permalink: /2012/03/22/ios-devices-not-getting-ipv6/
tags:
- planetcanonical
title: '[Resolved] iOS devices not getting IPv6'
---
Dear Lazyweb,

I'm at a loss here. I've got an IPv6 setup at home, with radvd giving out network information. And it basically just works. My laptop associates with the wireless network, sends out a Router Solicitation packet, the router responds with a Router Advertisement (RA), and the laptop gives itself an IP. And for good measure, the router sends out unsolicited RAs every 10 seconds or so.

But my iPhone and iPad are no longer getting IPv6 addresses. They associate with the AP, get a DHCPv4 address, then nothing. I've even tried using the [Ip6config](http://itunes.apple.com/us/app/ip6config/id408230297?mt=8) app to scan for RAs, but according to it, no RAs arrive. But I can see them from my laptop, capture them with Wireshark, and they look proper. And of course there's no way to get around this in iOS, since there are literally no user-configurable IPv6 options.

I know this worked at one point, because whenever I try searching the web, I keep coming up with [my own post from 2010 on getting this working](http://forums.macrumors.com/showthread.php?t=951542). That being "hey, it's not working... oh, now it is".

**Update (2012-06-08)**: [I've figured out what caused this.](http://www.finnie.org/2012/06/08/ipv6-in-apple-ios/)
