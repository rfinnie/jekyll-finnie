---
author: Ryan Finnie
categories:
- Uncategorized
date: 2010-10-08 15:24:59
guid: http://www.finnie.org/2010/10/08/im-on-a-boat-in-an-iana-document/
id: 1576
layout: post
permalink: /2010/10/08/im-on-a-boat-in-an-iana-document/
tags:
- 2ping
- planet:canonical
title: I'm on a boat^H^H^H^H^H^H^H^H^H in an IANA document.
---
In the spirit of [jwz's RFC announcement](http://jwz.livejournal.com/1305521.html), the IANA [assigned a registered port to 2ping](http://www.iana.org/assignments/port-numbers) this week! Starting with the next release, 2ping will officially be operating on port 15998/udp.

<pre><a href="http://www.finnie.org/software/2ping/">2ping</a>           15998/udp  2ping Bi-Directional Ping Service
#                          Ryan Finnie &lt;ryan&finnie.org&gt; 06 October 2010</pre>

A new release will probably be coming this weekend, which includes the default port number change. Also included will be a new opcode to allow the peers to specify host processing latency (the amount of time elapsed between a packet being received and a reply being sent out, measured in microseconds). The release will most likely jump from version 0.0.3 to 0.9.0. Then hopefully it'll just be some last minute work before a protocol finalization and a 1.0 release!
