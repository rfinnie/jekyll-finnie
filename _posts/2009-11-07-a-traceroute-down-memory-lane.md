---
author: Ryan Finnie
date: 2009-11-07 01:24:13
guid: http://www.finnie.org/2009/11/07/a-traceroute-down-memory-lane/
id: 1189
layout: post
permalink: /2009/11/07/a-traceroute-down-memory-lane/
title: A traceroute down memory lane
---
If you downloaded Red Hat Linux 9 from BitTorrent in April 2003, you probably (partly) have me to thank.

In 2002, I helped build a pair of datacenters in Reno, NV and Raleigh, NC. They were technologically wonderful, built to the same standards as Exodus datacenters. Lots of bandwidth, redundancy everywhere, futuristic security. One of the design principles was that we could take a potential client into a node room and say, "pull any two cables you'd like". With limited exception (downlink Ethernet to individual clients was only single redundancy) we succeeded, but never actually got to show that off. They were great datacenters, but we could barely sell anything, and they mostly went unused until the company's demise in 2004.

So we had massive amounts of bandwidth (by 2002 standards; two 155Mbps OC3s and a 45Mbps DS3 per site, provisioned from an OC12 that could effectively double the bandwidth in the future), and only a dozen clients, if that. I set up a mirror of as many Open Source projects as I could find, but that only ate up about 20Mbps on average.

On March 31, 2003, Red Hat Linux 9 was released. This was the first Red Hat product to be officially offered via BitTorrent. BitTorrent had been released about a year earlier, and was just starting to gain traction. RHL9 was arguably the first major worldwide test of a major software release. So hey, I had all this bandwidth, and there were thousands upon thousands of people downloading RHL9 through BitTorrent. I threw up a seed machine in the Reno datacenter, and watched the slurping begin.

Unfortunately, the official BitTorrent client could only seem to push about 50Mbps of torrent traffic before hitting a wall. So I added another server. And another. And another. And then 4 in Raleigh. All told, I was pushing 400Mbps of traffic across 8 machines and two datacenters into the mesh, and was often seeding at least half of the traffic of that single torrent. I believe that lasted for 3 or 4 weeks.

The company went out of business in 2004 (for non-BitTorrent-related reasons of course), and I was later hired by the company responsible for maintaining and eventually turning down the Reno datacenter. We had our network (and the remaining clients) migrated out of the datacenter by the middle of December, but due to a dispute with the eventual purchaser of the datacenter space, I was told to keep the datacenter "operational" through December 31, 2004. To do so, I left one Linux server running, connected to the only remaining transit (an OC3), and routed our remaining IP space to it, a /20. An entire /20.

So for 2 weeks at the end of 2004, I had a single server, sitting in the middle of a 50,000 square foot world-class datacenter, with a dedicated OC3 running directly to it, and 4096 IPs at my disposal. I actually can't remember what I ended up doing with it, except for putting up a web server with a test page that responded to all IPs. I'm sure it was more profound than that.

(In case you're wondering, Linux, at that time, did NOT like having 4096 sub-interfaces. They would all eventually work, but bootup would take at least 10 minutes. It seemed adding each new interface introduced a higher non-linear delay.)
