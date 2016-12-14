---
author: Ryan Finnie
categories:
- Uncategorized
date: 2011-09-25 15:26:13
guid: http://www.finnie.org/2011/09/25/introducing-twuewand/
id: 1908
layout: post
permalink: /2011/09/25/introducing-twuewand/
tags:
- planet:canonical
title: Introducing twuewand
---
[twuewand](http://www.finnie.org/software/twuewand/) is a true [hardware random number generator](http://en.wikipedia.org/wiki/Hardware_random_number_generator), written in about a hundred lines of Perl.

No, really.

You can stop laughing now.

First, a little history. This is greatly simplified, and specific to Linux, but the concepts are somewhat universal. Linux has three entropy pools. The first is a hidden, primary entropy pool that directly or indirectly receives entropy from several main sources, described later.

The secondary pool feeds from the primary pool, and is used to drive /dev/random. /dev/random is blocking, meaning if both the primary and secondary entropy pool exhaust, reads from /dev/random block until more entropy is generated.

The third pool is the urandom pool, and functions almost exactly as the secondary pool, but drives /dev/urandom. The key difference is while the urandom pool can draw from the primary pool, it can also reuse entropy to avoid blocking in the case of pool exhaustion.

Now, entropy is gathered from several sources to directly feed the primary pool: keyboard and mouse timings, interrupts, disk activity, and entropy fed back from the other two pools, directly or indirectly. However, consider a server. Most of the time it receives no keyboard or mouse activity, and the interrupts and disk activity are theoretically predictable. But the primary pool can also be influenced by writing to the other pools, and modern Linux distributions take advantage of this. Upon shutdown, a number of bytes are read from /dev/urandom (usually 4096 today) and written to a state file. When the computer is booted again, the OS reads this file and writes the bytes back to /dev/urandom. This isn't exactly completely restoring the state pre-shutdown; remember there are other sources of entropy (including the disk activity needed to read the file), so writing the same 4096 bytes back to the urandom pool merely influences the urandom pool and the primary pool, resulting in entropy that is unpredictable from boot to boot.

Now, consider a LiveCD or a diskless workstation. Without the ability to introduce dynamic entropy from a previous session, the predictability increases a lot. If the computer had a hardware random number generator, we wouldn't have this problem. The hardware RNG could be queried directly, or it could be used to influence a pseudo RNG like the system Linux uses. But very few computers have hardware RNGs, and almost zero consumer-level computers do.

Or do they? Every computer actually has two hardware random number generators, which can be combined to get a stream of random numbers. They are the CPU itself and the real-time clock (RTC). 

twuewand is a truerand implementation, first invented in 1995 by D. P. Mitchell. It relies on the fact that the CPU and RTC are physically separate clocked devices, and therefore time and work are not linked. twuewand's operation is very simple. It sets an alarm for sometime in the future (by default 4 milliseconds, as determined by the RTC), and then starts flipping a bit between 0 and 1 (work performed by the CPU). When the alarm is reached, the bit is taken. Voilà, random bit. It then repeats this process for as many bytes as needed.

This process produces a stream of truly random bits. An attacker can alter the amount of work performed by the CPU by introducing his own work during the same time period, but it still does not affect the output in a predictable way. However, this stream is still prone to bias. So after a certain number of bytes are collected, it is run through a cryptographic hash digest, by default SHA512, or MD5 if Digest::SHA is not installed. The hashed data is then output. This "whitens" the data, hopefully decreasing bias while retaining randomness.

twuewand could be used as a primary source of random data, but its primary purpose is intended to be an entropy pool seed. In Linux, you would execute:

> <pre>twuewand $(cat /proc/sys/kernel/random/poolsize) &gt;/dev/urandom</pre>

I wrote twuewand a few weeks ago when I first learned of truerand. truerand is an interesting concept, but it's actually almost never used in the real world anymore. The reason it was invented was to add another source of entropy to entropy pools, but the discovery of the benefits of saving pool data to reintroduce after reboot mostly made it unnecessary. But remember, this source is not available to LiveCDs and diskless workstations. I wrote twuewand for use by [Finnix](http://www.finnix.org/) during startup, but hit a major snag. Namely, it's slow. Each bit takes a minimum of 4ms to generate, and that adds up. Generating 4096 bytes takes over 2 minutes. So I'm not going to have Finnix run it during startup, at least not for the full 4096 byte pool size. Perhaps 8 bytes by default, which will take a little over a quarter of a second. It's not as cryptographically secure as filling the entire pool, but it's better than nothing. Either way, twuewand will at least be available in the next version of Finnix if you desire to use it.

(If you don't get the "twuewand" name reference, go watch [The Princess Bride](http://en.wikipedia.org/wiki/The_Princess_Bride_%28film%29).)
