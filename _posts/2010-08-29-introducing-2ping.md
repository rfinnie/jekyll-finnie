---
author: Ryan Finnie
categories:
- Uncategorized
date: 2010-08-29 18:28:27
guid: http://www.finnie.org/2010/08/29/introducing-2ping/
id: 1489
layout: post
permalink: /2010/08/29/introducing-2ping/
tags:
- 2ping
- planet:canonical
title: Introducing 2ping
---
[2ping](http://www.finnie.org/software/2ping/) is a bi-directional ping utility. It uses 3-way pings (akin to TCP SYN, SYN/ACK, ACK) and after-the-fact state comparison between a 2ping listener and a 2ping client to determine which direction packet loss occurs.

No wait, that sounds too dry. I've been writing dry documentation and protocol references for weeks.

2ping is like ping, but does **magic**.

Has this ever happened to you?

> <pre>PING 10.0.0.1 56(84) bytes of data.
64 bytes from 10.0.0.1: icmp_seq=1 ttl=255 time=0.062 ms
64 bytes from 10.0.0.1: icmp_seq=4 ttl=255 time=0.064 ms

--- 10.0.0.1 ping statistics ---
4 packets transmitted, 2 received, 50% packet loss, time 3999ms
rtt min/avg/max = 0.062/0.063/0.064 ms</pre>

Great, two replies were never received. But why? Did the far end receive the requests and reply? There's no way to know. Enter 2ping.

> <pre>2PING 10.0.0.1: 64 to 512 bytes of data.
64 bytes from 10.0.0.1: ping_seq=1 time=0.083 ms
Lost inbound packet from 10.0.0.1: ping_seq=2
Lost outbound packet to 10.0.0.1: ping_seq=3
64 bytes from 10.0.0.1: ping_seq=4 time=0.041 ms

--- 10.0.0.1 2ping statistics ---
4 pings transmitted, 2 received, 50% ping loss, time 4005ms
1 outbound ping losses (25%), 1 inbound (25%), 0 undetermined (0%)
rtt min/avg/max = 0.041/0.062/0.083 ms
6 raw packets transmitted, 2 received</pre>

Now THAT tells you a lot more. One ping was received by the server and replied to (but you never received the reply), and another was never received by the server. Assuming you trust the other side to have a stable network connection, that suggests both flaky inbound and outbound problems toward your side.

## How does it work?

[<img src="/blog-media/2010/08/2ping-lolcat-300x225.jpg" alt="" title="2ping-lolcat" width="300" height="225" class="aligncenter size-medium wp-image-1511" srcset="/blog-media/2010/08/2ping-lolcat-300x225.jpg 300w, /blog-media/2010/08/2ping-lolcat.jpg 640w" sizes="(max-width: 300px) 100vw, 300px" />](/blog-media/2010/08/2ping-lolcat.jpg)

Normal ping uses ICMP. It is assumed that nearly all IP-capable machines are listening for ICMP ping requests, and will respond with an ICMP response.

2ping is a combined client and listener utility that communicates over UDP. You start a listener on the far end (<tt>2ping --listen</tt>), then on the near end you start the client mode and tell it to go to the far end. In normal conditions, it operates much like a regular ICMP ping.

> Client: "Ping!"
  
> Listener: "Pong!"

Actually, it's a little more involved. By default, 2ping uses 3-way pings:

> Client: "Ping!"
  
> Listener: "Pong! And Ping!"
  
> Client: "Pong! And that last ping took 1.2ms to complete!

This allows the listener to gain a little more insight into what's happening on the client end. It knows what the RTT is between the 2nd and 3rd legs, and in addition, the client let the listener know the result of the ping between the 1st and 2nd legs. Here's what 2ping looks like in listener mode:

> <pre>2PING listener (0.0.0.0): 64 to 512 bytes of data.
64 bytes from 10.0.0.2: ping_seq=1 time=0.226 ms peertime=0.077 ms
64 bytes from 10.0.0.2: ping_seq=2 time=0.166 ms peertime=0.035 ms
64 bytes from 10.0.0.2: ping_seq=3 time=0.164 ms peertime=0.032 ms
64 bytes from 10.0.0.2: ping_seq=4 time=1.073 ms peertime=0.040 ms</pre>

Nifty, huh? Now the real magic happens when packet loss starts occurring. Say a few replies from the listener to the client never arrive at the client. Communication isn't totally dead between the two hosts, and they do eventually start talking to each other. When this happens, they start comparing notes:

> Client: "Ping #1!"
  
> Listener: "Pong #1!"
  
> Client: "Ping #2!"
  
> Client: "Ping #3!"
  
> Listener: "Pong #3!"
  
> Client: "Ping #4! Oh, and I never received a response to ping #2."
  
> Listener: "Pong #4! I received ping #2 and replied. Must be inbound packet loss on your side."

(This is a simplified example that doesn't show the extra communication with the 3-way pings. But the principle is the same.)

Again, this assumes a stable listener's network. "Must be inbound packet loss on your side" doesn't mean much if you can't assume that it wasn't lost outbound from the listener's perspective.

## Current status

[Go and download version 0.0.1.](http://www.finnie.org/software/2ping/) This version is mostly feature-complete, and implements many of the relevant command-line options from the regular ping utility ("2ping -a" for audible ping, etc). [A 2ping manpage is partially complete](http://www.finnie.org/software/2ping/2ping.1.html), and documents all of the command-line options.

2ping is written in Perl, and all of the base module requirements should be in any default Perl install. It is IPv6 compatible with the "-6" switch, but requires IO::Socket::INET6, which is probably not included by default. But 2ping only tries to load it if you are using the "-6" switch, so the module is not needed if you do not need IPv6 support.

This is a VERY early release. The code is horrible and not documented. The command-line switches may change in the future, the wire protocol may change in the future, and the UDP port used by default (currently 58277) definitely WILL change in the future. (I've sent the draft 2ping protocol to the IANA for review, and hopefully they will assign a registered port for 2ping use.) A version 1.0 release will not be made until the code is cleaned up, the final UDP port is assigned, and the protocol is finalized.
