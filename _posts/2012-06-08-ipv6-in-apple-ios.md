---
id: 2099
title: IPv6 in Apple iOS
date: 2012-06-08T17:15:58+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/2012/06/08/ipv6-in-apple-ios/
permalink: /2012/06/08/ipv6-in-apple-ios/
categories:
  - Uncategorized
tags:
  - planet:canonical
---
I solved [my problem from a few months ago](http://www.finnie.org/2012/03/22/ios-devices-not-getting-ipv6/) with my iPhone and iPad not being able to get IPv6 info. It was actually a combination of two problems.

Last year I bought a Netgear N750DB router as an access point to replace my Linksys WRT54GL. Turns out the N750DB will send multicast traffic from a wireless device back to itself. This breaks IPv6 Duplicate Address Detection (DAD), and I was also seeing problems on my Ubuntu laptop. So I went back to the WRT54GL, and while stateless autconfiguration started working on the laptop again, it was still not working on the iPhone and iPad.

The second part was the iOS devices needed to be rebooted. No idea why, but after a hard reboot, they started pulling RA (and DHCPv6, which I had configured in the meantime). I've since bought a Linksys E4200 v1 to replace the N750DB, and everything is now working great.

Note that it's actually hard to tell when iOS has an IPv6 address. There are literally no IPv6 options in iOS, and the IPv6 address itself is not displayed anywhere. The only hint is in the network config page. If you have a DHCPv6 server or giving out RDNSS via RAs, the IPv6 nameserver and/or domain will show up in "DNS" and "Search Domains", though will likely be cut off. The only OS-level way to get meaningful IPv6 address/route information is a utility app called [IPv6 Toolkit](http://itunes.apple.com/us/app/ipv6-toolkit/id440597511?mt=8) ($2 but worth it in this case). You can also go to a web site like [vsix.us](http://vsix.us/) ([which of course I wrote](http://www.finnie.org/2012/03/25/vsix-us-your-ip-address-with-a-few-tools/)) to see what your IP address is.

Note that even after fixing my AP and rebooting my devices, when I went to vsix.us in Safari, it was still preferring v4 over v6. I was able to solve that by clearing all cached info in Safari.
