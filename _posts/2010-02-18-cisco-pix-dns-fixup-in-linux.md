---
author: Ryan Finnie
date: 2010-02-18 19:12:42
guid: http://www.finnie.org/2010/02/18/cisco-pix-dns-fixup-in-linux/
id: 1259
layout: post
permalink: /2010/02/18/cisco-pix-dns-fixup-in-linux/
title: Cisco PIX DNS fixup in Linux?
---
At work we have a Cisco PIX firewall for the office. It's decent (if a bit eccentric; that is, hard to configure), but occasionally I go through a thought exercise to see how this firewall could be replaced with a Linux firewall. Most of the functionality is easy in Linux (NAT, ACLs, VPNs, etc), but one thing I get hung up on is DNS fixup. Fixup is a monitoring service much like nf\_conntrack/nf\_nat in Linux, and in DNS fixup's case can rewrite responses depending on the context. Here's an explanation:

The players:
  
- Mallory is the PIX firewall, with the 10.0.0.0 network inside and the 9.9.9.0 network outside. (Despite conventional naming examples, Mallory is not malicious here, but otherwise has the same attributes.)
  
- Alice is the DNS server, 10.0.0.2 inside, 9.9.9.2 outside. Alice knows only about internal IPs in her DNS database.
  
- Bob is some server, 10.0.0.3 inside, 9.9.9.3 outside. Bob is listed with Alice as bob.corp.example.com, 10.0.0.3.
  
- Charlie is a client on the outside network.
  
- Dave is a client on the inside network.

Now, say Charlie (outside) queries bob.corp.example.com via Alice's external IP. Alice will respond with 10.0.0.3. Mallory intercepts the response, knows that Bob is 10.0.0.3 on the inside and 9.9.9.3 on the outside, so she rewrites the response as 9.9.9.3 and gives it to Charlie.

It also works in the opposite direction. Say www.example.com is a web server served by Bob, and DNS is hosted by an outside DNS provider which obviously returns 9.9.9.3 for www.example.com. Now say Dave (inside) queries www.example.com via Alice. Alice doesn't know about www.example.com, so she goes out to the Internet (through Mallory) to find it. The outside DNS responds with 9.9.9.3. Again, Mallory knows about Bob's mapping and will rewrite the response to 10.0.0.3 to Alice, which then gives the final answer to Dave.

As far as I know, there is nothing in Linux to facilitate this. Yes, I know about split-horizon DNS, but it's a pain to maintain multiple zone copies, and Alice's DNS service would have to be moved to Mallory directly. The PIX does this all automatically for you (if you want; of course it can be disabled).

(Please, prove me wrong.)
