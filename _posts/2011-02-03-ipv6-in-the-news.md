---
id: 1709
title: IPv6 in the news
date: 2011-02-03T22:44:21+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/?p=1709
permalink: /2011/02/03/ipv6-in-the-news/
categories:
  - Uncategorized
tags:
  - planet:canonical
---
"Zero day" arrived today, with [the IANA delegating the last /8s to the RIRs](http://www.nro.net/news/ipv4-free-pool-depleted). Now, this doesn't spell the immediate end of the Internet, but it does bring some more public light to the fact that, while the problem has been know for at least a decade, there has been almost no effort to do anything about it at a populace level. Unlike Y2K, there wasn't a race to a certain date that would spell disaster if work was not done. Instead, from this day on, things will get incrementally harder.

At this point, [about 500,000 /24s are left at ARIN](http://ipv6.he.net/), the North American RIR. (Other RIRs are of course affected, but I'm focusing on North America here.) Over the next year or so, I predict the following will start to occur:

  * Hosting/transit providers will start to demand stricter justification for IPv4 addresses. Years ago, you didn't need any justification. Today, it's pretty easy to BS justification, with little proof you need extra addresses. Maybe tomorrow your provider will actually verify that you need that second IP for an SSL site. (SSL is one of the few reasons you would need additional IPs in a hosting environment.)
  * The "commodity" price of IPv4 addresses at the hosting level will start to go up. In general, IPv4 addresses cost about $1/IP/month at this point.
  * Consumer ISPs will start switching to [Carrier Grade NAT](http://en.wikipedia.org/wiki/Carrier_Grade_NAT). NAT (PAT in this case, technically) is what allows your home router to handle multiple home computers with only one ISP-supplied global IP. Carrier Grade NAT simply moves the point where NAT occurs up to the ISP level, so they can serve more customers with fewer global IPs. The fact is 99% of typical home users don't need global IP addressing to the door. Of course, I'm not one of those 99%, and I hope the "good" ISPs who eventually move to this model will allow for global IP addressing for customers with justification. (Hey, I said these things were inevitable, not that I supported them.)
  * [World IPv6 Day](http://isoc.org/wp/worldipv6day/), later this year, will be another one of those "eye-opener" days, similar to today. It will have an immediate short-term backlash from the small percentage of users with badly configured network gear who cannot reach Google/Facebook/Yahoo/etc. (The purpose of the Day is to test the current state of the Internet with regard to systems that malfunction when going to sites that provide both IPv4 and IPv6 addresses, whether they have IPv6 or not. Many smaller sites already run dual-stacked, this site included.) Despite the immediate backlash, there will be a short-term interest in IPv6 site deployment as evidenced by the vast majority of users who did NOT malfunction.
  * More consumer ISPs and web sites will start to implement dual-stacked IPv6.
  * Disruption will occur over time, but that's fine. Frankly the only thing driving adoption up until this point was curiosity, and after this point, it's disruption or fear of disruption.
  * There will eventually be a tipping point where IPv6 availability passes, say, 75% of IPv4 availability. At this point, new web sites and network services will slowly start being deployed IPv6-only. Again, I'm willing to bet we're still more than a few years off at this point. I saw a similar tipping point happen in the late 90s with the availability of name-based virtual web hosting. By 2000, it got to the point when it was relatively safe (but not universal) to deploy name-based virtual hosts.
  * Once that starts happening, it will provide leverage for the remaining IPv4-only services to quickly adopt IPv6. Again, in the historical name-based virtual hosting example, users who couldn't get to a new web site because of an old web browser would need to upgrade their browser. In this case, consumers will start demanding ISPs who haven't migrated to do so. The difference between examples is here the content providers (web sites, etc) will also be leveraged.

At that point... Bam! IPv6 adoption. We're only looking at another... oh, decade of work. And frankly, it's something that should have started a decade ago.

Well, that was a little long. I intended that to be a short lead-in to what I've personally been doing with IPv6 recently, but I'll follow up with that in the next post.