---
id: 1339
title: 'IPv6: Living in the future, etc, etc'
date: 2010-04-30T19:48:07+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/?p=1339
permalink: /2010/04/30/ipv6-living-in-the-future-etc-etc/
categories:
  - Uncategorized
---
Well, it's 2010, and I'm finally living in The Future. I now have an ISP that is offering native IPv6, [my colo provider](http://vr.org/). I switched over my irssi connections to make sure all IPv6-capable IRC networks are connecting via IPv6. (OFTC, Foonetic, Coldfront, Freenode -- sadly, Slashnet is the only one without an IPv6 server.) I have also added AAAA records to many of my hosted domains, such as [finnie.org](http://www.finnie.org/), [finnix.org](http://www.finnix.org/), [x11r5.com](http://www.x11r5.com/), [velociraptors.info](http://www.velociraptors.info/) and [hampr.com](http://www.hampr.com/). Also, I can reasonably assume I am running the world's only IPv6 [TCPMUX server](http://www.finnie.org/2010/02/13/in-tcpmuxd-a-secure-rfc-compliant-tcpmux-server/).

While I do not have native IPv6 at home, I am using [Hurricane Electric's Tunnel Broker](http://www.tunnelbroker.net/) service. I'm not going to go into details on how to set this up, but I do want to stress the importance of firewalling IPv6. In the IPv4 world, NAT is used as a security crutch. In "The Future", when everything is IPv6, NAT will be irrelevant, but because of that, firewalling is that much more important. If you use a IPv6 tunneling service, be sure your internal LAN (which then becomes an external LAN) is properly firewalled.

In Linux, this requires the use of ip6tables. I personally use a simple setup, allowing outbound traffic, inbound ICMP, inbound SSH and a few select inbound services to individual machines. I've included my ip6tables config below.

<pre># Set a default DROP policy.  Note that this only affects IPv6 traffic,
# it does not affect the regular iptables FORWARD policy.
<strong>ip6tables -P FORWARD DROP</strong>
# Allow any outbound traffic from your local LAN (2001:470:1f05:22e::/64). 
# Replace "hetunnel" with your tunnel/outbound interface (or leave it off,
# though it helps prevent possible spoofing).
<strong>ip6tables -A FORWARD -s 2001:470:1f05:22e::/64 -o hetunnel -j ACCEPT</strong>
# Allow any established inbound or outbound traffic.
<strong>ip6tables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT</strong>
# Allow ICMP inbound.  "ipv6-icmp" is required for ip6tables here.
<strong>ip6tables -A FORWARD -p ipv6-icmp -j ACCEPT</strong>
# Allow SSH inbound to any host.
<strong>ip6tables -A FORWARD -p tcp -m tcp --dport 22 -m state --state NEW -j ACCEPT</strong>
# Allow µTorrent inbound tcp/udp, to my Vista machine in this case.
<strong>ip6tables -A FORWARD -p tcp -d 2001:470:1f05:22e:24c3:ff01:e72a:3487 \
  --dport 30173 -m state --state NEW -j ACCEPT
ip6tables -A FORWARD -p udp -d 2001:470:1f05:22e:24c3:ff01:e72a:3487 \
  --dport 30173 -m state --state NEW -j ACCEPT</strong></pre>
