---
id: 2108
title: IPv6 autoconfiguration in a nutshell
date: 2012-06-10T02:39:25+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/?p=2108
permalink: /2012/06/10/ipv6-autoconfiguration-in-a-nutshell/
categories:
  - Uncategorized
tags:
  - planet:canonical
---
[ This was written at 2AM, and I tended to ramble. Hopefully I'll come back tomorrow and clean it up, add links to RFCs and Wikipedia entries where relevant, etc. ]

I've been explaining this a number of times in the last few days, so I figured I'd get this down in blog form to avoid repeating myself (even more). IPv6 is much like IPv4 in most respects, but its autoconfiguration can seem rather alien to people already familiar with IPv4 (that is, DHCP).

On an IPv6 network, the default router runs a service called the Router Advertisement daemon. Its primary purpose is to send out packets saying "hey, I'm the default router for the 2001:db8:1234:abcd::/64 network".

It actually doesn't matter what the router's IPv6 address actually is on 2001:db8:1234:abcd::/64; indeed, it (usually) doesn't even tell your what it is. Instead, it sends out RAs using its "link-local" fe80:: address. Link-local addresses are specific to the LAN segment you're on, and every device is required to have one (derived from the MAC address). It's sort of a middleware layer of doing things, between Layer 2 and Layer 3. But the important thing is you can talk to other devices if you know their link-local fe80:: address (and you're both on the same segment), and you can route to them.

So it really doesn't matter if the router's address is 2001:db8:1234:abcd::1/64 or 2001:db8:1234:abcd::dead:beef/64 or if it doesn't even have an address; if you're doing autoconfiguration, your OS will most likely be routing to it based on its link-local address. For example, my home router's link-local address on the LAN side is fe80::6a05:caff:fe02:4e4d/64, which it uses to send RAs.

By default the router sends an RA every minute or so, so the client device could theoretically set up autoconfiguration without ever talking to the router. But it will also respond to Router Solicitation requests being broadcast to the network. These requests are responded to (via broadcast) with the exact same RA packet it would have sent on a timer, except this time it happened on-demand.

Now, the RA lets the client device know what the LAN's IPv6 network space is. It also sends a series of flags which can make autoconfiguration possible.

The most relevant is the Autonomous address-configuration ("A") flag. This flag provides stateless autoconfiguration, and essentially says "you are permitted to construct your own IP address in the network I mentioned". The client will then use the MAC address of the interface as a basis for constructing an IP address. In the above example, 2001:db8:1234:abcd::/64 was the network. Say my interface's MAC address is 00:19:d2:43:c0:c4. Using the recommended MAC-munging method, this becomes 2001:db8:1234:abcd:219:d2ff:fe43:c0c4/64.

The client then uses Duplicate Address Detection (DAD) to make sure nobody on the network is already using this address. (DAD is also used to make sure the link-local address mentioned above is also unique to the network segment.)

So that's it! The client now has enough information to construct IP information, and in theory it never even had to send a packet. (I say "in theory" because DAD requires sending out a multicast packet. Plus, most OSes send out a Router Solicitation to trigger the Router Advertisement.) In this example, the router, fe80::6a05:caff:fe02:4e4d, sent out an RA stating the network is 2001:db8:1234:abcd::/64, and the "A" flag was set. The client used this information to construct the IP 2001:db8:1234:abcd:219:d2ff:fe43:c0c4/64, which it gave to itself, with the default route being fe80::6a05:caff:fe02:4e4d.

Now what about DNS? In a dual-stack environment, the client probably already has an IPv4 DNS address by now, and can certainly use that to look up IPv6 (AAAA) records. But for pure IPv6, there are two methods for getting DNS servers to the client.

First up is RDNSS, a relatively recent extension to the RA standard. It's simply an extension which gives one or more DNS servers in the RA packet. Simple, but not often used today.

The RA also has a "Managed address configuration" ("M") flag. This simply means, "there is a DHCPv6 server on this network. You may go look for it for more granular control." Also known as stateful autoconfiguration.

DHCPv6 is similar to DHCPv4, but has a number of differences. I'm not going to go into it in detail, but you can do pretty much everything you can do with DHCPv4: pooled address allocations, individual address allocation, DNS servers, domain search lists, etc. It's worth noting that if you have both the "A" and "M" flags in the RA set (and you're running a DHCPv6 server), the client will use both of these methods. That is to say, it'll construct an IP address using stateless means, then request an IP address from the DHCPv6 server, and will bind both of these addresses to the interface.

Side note: IPv6 has no concept of network or broadcast addresses. In this post's example, 2001:db8:1234:abcd::/64 was the network, but 2001:db8:1234:abcd:: can also be a device. However, most networks I've seen continue the old tradition of giving the .1 (or in IPv6's case, :1) address to the default router.