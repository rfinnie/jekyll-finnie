---
date: 2011-02-10 00:20:53-08:00
layout: post
tags:
- planetcanonical
title: IPv6 glue records added
wp_id: 1739
---
My [domain transfer from Dotster to Gandi]({{ site.url }}{{ site.baseurl }}{% post_url 2011-02-03-ipv6-in-the-finnie %}) completed today, giving me the ability to add IPv6 glue records for my nameservers. With that in place, end-to-end IPv6 is now possible for my colo services:

<pre>; &lt;&lt;>> DiG 9.7.1-P2 &lt;&lt;>> +trace +additional -6 -t AAAA colobox.com @2001:470:1f05:22e::1
;; global options: +cmd
.			49755	IN	NS	a.root-servers.net.
a.root-servers.net.	53372	IN	AAAA	2001:503:ba3e::2:30
;; Received 508 bytes from 2001:470:1f05:22e::1#53(2001:470:1f05:22e::1) in 2 ms

com.			172800	IN	NS	a.gtld-servers.net.
a.gtld-servers.net.	172800	IN	AAAA	2001:503:a83e::2:30
;; Received 501 bytes from 2001:503:ba3e::2:30#53(a.root-servers.net) in 49 ms

colobox.com.		172800	IN	NS	feh.colobox.com.
feh.colobox.com.	172800	IN	AAAA	2607:f740:0:d::2
;; Received 159 bytes from 2001:503:a83e::2:30#53(a.gtld-servers.net) in 54 ms

colobox.com.		3600	IN	AAAA	2607:f740:0:d::2
colobox.com.		3600	IN	NS	feh.colobox.com.
feh.colobox.com.	3600	IN	AAAA	2607:f740:0:d::2
;; Received 187 bytes from 2607:f740:0:d::2#53(feh.colobox.com) in 66 ms</pre>

(Records unrelated to the trace have been omitted for readability.)

It also gave me the ability to complete [Hurricane Electric's IPv6 certification](https://ipv6.he.net/certification/scoresheet.php?pass_name=rfinnie):

<a href="https://ipv6.he.net/certification/scoresheet.php?pass_name=rfinnie" target="_blank"><img src="https://ipv6.he.net/certification/create_badge.php?pass_name=rfinnie&badge=2" width=250 height=194 border=0 alt="IPv6 Certification Badge for rfinnie"></img></a>

Also, last week I found out that my work office's ISP has native IPv6 connectivity, and they set us up with an allocation this week, a /64 for the border VLAN between our networks and a /48 routed to our router/firewall. It's up and running, though the only internal address I've set up so far is my workstation.

Future. Here.
