---
date: 2024-05-13 09:49:11-07:00
excerpt: '... but I didn''t report it. Why not?'
excerpt_standalone: true
layout: post
title: I discovered the Debian OpenSSL bug
---
16 years ago today, [Debian announced CVE-2008-0166](https://lists.debian.org/debian-security-announce/2008/msg00152.html), "predictable random number generator". Reported by and discovery attributed to Luciano Bello, this was a Debian-specific OpenSSL vulnerability which had been in place for nearly two years and is [still being exploited in the wild to this date](https://16years.secvuln.info/).

However, in this post I'll tell the story of how I discovered the bug nearly a year earlier, but didn't report it. Why didn't I? Read on for the story behind this totally-not-clickbait title!

---

[Lessons from the Debian/OpenSSL Fiasco](https://research.swtch.com/openssl), written a week after the announcement, goes into more detail, but basically the bug resulted in key generation being tied directly to the process ID (PID) of the generating program. This would be bad enough today with most distributions defaulting to 4.2 million PIDs, but back in 2008, the default on all distros, including Debian, was 32,768. I believe Linux had optional higher PID support back then (though the earliest I can find reference to is 2011 from a quick search), but it's been only recently that higher PID support has been default on distros, since the assumption of a maximum PID being 32,768 was baked into a lot of software for so long.

On April 8, 2007, Debian 4.0 "etch" was released. This was the first stable release containing the OpenSSL bug, but it had been available in Debian unstable/testing since September 17, 2006, as well as a few non-LTS Ubuntu releases, and possibly other Debian-based distributions.

At the time, I was working for a marketing company in Reno, Nevada. A large part of our business was website design, and we did in-house hosting to support it. It was large enough that we had datacenter space and multiple sysadmins to support the programmers and designers. We started out small with mostly shared hosting, with dedicated servers for the largest customers. But around 2005, I developed an internal system for virtualized hosting based on User Mode Linux. UML allowed for running a kernel as a user-mode program on the host system. This provided decent separation of the guests, and was used by early Virtual Dedicated Server providers like Linode, before they moved on to Xen and later KVM.

So we were using UML on new guests, but were still pretty much treating them like dedicated servers. And back then, that meant the time-honored tradition of a quick-and-dirty initialization script to automate new setup tasks. I would boot the guest with a generic base Debian image, connect to its virtual serial console, log in as root, and run the equivalent of `curl | sh`. (At least the script being retrieved was on a local LAN.) The script would ask a few questions, then run all our base setup installations and configurations.

A few months after the release of etch, I noticed that many of the deployed guests had the same OpenSSH host key. Over half of all deployed etch guests had the exact same key, but there were also smaller clusters of guests which shared other host keys between them. I had no idea how this had happened, and worse, I wasn't able to reproduce the problem when deploying test guests. Eventually I gave up on trying to root cause the issue, re-generated the duplicated SSH host keys, and moved on.

Then, on May 13, 2008, I saw the Debian announcement and it all made sense. I knew what I had discovered last year, why it had happened, and I also knew why I wasn't able to reproduce it.

One of the first things my deployment script did was `apt-get install openssh-server`. Because this happened very early in the script process, and because the base boot image was so minimal, the number of processes being run between kernel start and the point `ssh-keygen` is run as part of the OpenSSH server install was static, as long as my actions of logging in for the first time and running `curl | sh` were the same each time. When I was trying to reproduce the problem after discovering it, I was introducing variances by, say, checking the environment ahead of time, or by manually downloading the script and then running it, or by editing the script itself to add debugging.

This explained why most of the deployed guests had the same host key. The other clusters of guests with a shared keys were explained by things like making a change to the install script down the road, or even typing a fidget `ls` or similar before starting the install. I had discovered the symptoms of the Debian OpenSSL bug months ahead of time, but I didn't report it because I hadn't realized the cause or implications of what I had discovered, and I'm still kicking myself for not putting the pieces together.

Checking back on my IRC logs from the day after the announcement revealed this humorous reply as I was telling the story:

> Way to not report this last year and save the day
>
> Next time you discover a critical bug, save it for christmas or new years eve though
