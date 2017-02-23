---
date: 2012-05-30 01:42:15-07:00
layout: post
tags:
- planetcanonical
title: apt-get dist-upgrade
wp_id: 2079
---
This weekend, I upgraded the OS on my main colo box, from 32-bit Debian lenny to 64-bit Ubuntu 12.04 precise LTS. This was an installation which I've had for over 8 years. It was originally installed in early 2004, when Debian sarge was in its long beta freeze. It had been dist-upgraded over the years, from sarge to etch to lenny, and was quite well maintained. And over the years, the hardware has been upgraded, from a Pentium 4 with 512MB memory and 80GB usable disk, to a Pentium D with 2GB memory and 160GB disk, to a Core i7 with 9GB memory<sup>[1]</sup> and 320GB disk.

However, it was still an 8-year-old installation, and I wanted to get to a 64-bit OS. Debian lenny's support was dropped in February, and Ubuntu precise seemed like a good target, so I made the decision late last year to replace the OS.

I bought two 1.5TB WD Black SATA drives and installed precise on them a few weeks ago, in a RAID1 set. My colo box in San Jose has a 3ware 9650SE 2-port SATA 2 card, and I had an identical card available at home. Thankfully 3ware cards store RAID configuration on the disks themselves, so it was just a matter of installing the OS on the surrogate server at home and shipping the disks to San Jose to be physically installed. I also bumped the memory up to its maximum of 24GB, as long as the case was open (memory is insanely cheap right now).

The precise install was a minimal install, with just networking and SSH configured. I then took a backup of the lenny install and put it in /srv/oldos. The idea was once the datacenter swapped out the disks and powered it on, I'd go in and, for example, "<tt>chroot /srv/oldos /etc/init.d/apache2 start</tt>" for all the essential services. I could then migrate the services into the new install one at a time.

With a few small bumps<sup>[2]</sup>, that strategy worked well. However, a LOT of stuff is on this box, and it took me a most of the weekend to get everything right. Here's a sample of what this server is responsible for:

  * HTTP/HTTPS (Apache, a dozen or so web sites, plus MySQL/PostgreSQL)
  * SMTP (Postfix, plus Mailman, Dovecot IMAPS)
  * DNS (BIND9)
  * XMPP (ejabberd)
  * BitTorrent (bittornado tracker, torrent clients for [Finnix](https://www.finnix.org/))
  * Minecraft
  * Mumble/Murmur
  * rsyncd (Finnix mirrors)
  * [2ping](https://www.finnie.org/software/2ping/) listener
  * The all-important [TCPMUX](https://www.finnie.org/2010/02/13/in-tcpmuxd-a-secure-rfc-compliant-tcpmux-server/) server
  * Various AI, TTS, text manipulation and audio utilities for [X11R5](https://www.x11r5.com/) and the [Cybersauce Broadcasting Corporation](https://www.x11r5.com/radio/)

As of Monday, everything is now running off the new precise install. I installed this server over LVM, and have only allocated about 500GB of the 1.5TB usable space. There is a relatively small 120GB root partition, a 160GB /home partition, and a 250GB /srv partition. The idea is not much more than the OS should be on root (and should never need more than 120GB), /home is self explanatory, and all projects (web sites, etc) go in /srv. If /home or /srv need to be expanded, it can be done remotely relatively easily by umounting them, expanding the partitions using LVM, and running resize2fs. I've also left most of the space unallocated in case I want to run KVM guests some day, though I don't have an immediate need.

* * *

<sup>[1]</sup> Yes, 9GB memory. Triple channel, so it was 3x2GB + 3x1GB modules.
  
<sup>[2]</sup> I had two significant problems with the migrations:</p> 

  1. The problem with picking a custom third-party Festival voice for the voice of the Cybersauce Broadcasting Corporation is that it looks like it has not been updated in years, and is specific to only a handful of Festival versions. So now I have a minimal, yet 300MB chroot running (64-bit) Debian lenny for the sole purpose of providing Festival's text2wave utility with the custom voice.
  2. I'm using two packages<sup>[3]</sup> not available in Debian/Ubuntu: a Perl module (AI::MegaHAL) and a PHP module (crack), both of which are published at [my PPA](https://launchpad.net/~fo0bar/+archive/colobox). Both are sufficiently ancient and have not been updated upstream for years. Both compiled fine in 64-bit precise and at first appeared to run fine, but would then crash. Turns out both were not 64-bit safe, and required patching. Thankfully this is easy to roll out with PPAs.

<sup>[3]</sup> Okay, three, technically. I've packaged the tw_cli utility for the 3ware RAID card, but cannot distribute the package through my PPA because it's a non-free binary.
