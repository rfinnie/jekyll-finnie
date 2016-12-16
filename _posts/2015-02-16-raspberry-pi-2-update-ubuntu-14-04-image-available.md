---
date: 2015-02-16 22:41:02-08:00
layout: post
tags:
- planetcanonical
title: Raspberry Pi 2 update - (unofficial) Ubuntu 14.04 image available
wp_id: 2426
---
[Download the lastest Ubuntu 14.04 Raspberry Pi 2 image](https://wiki.ubuntu.com/ARM/RaspberryPi)

If you downloaded an older image than the current one, you shouldn't need to reinstall, but be sure to review the changelog in the link above.

_Note that this blog post originally contained a bunch more information, which has been moved to a [dedicated page on wiki.ubuntu.com](https://wiki.ubuntu.com/ARM/RaspberryPi)._

_I've closed comments on this blog post. If you are looking for help, please see [this post on the raspberrypi.org forums](https://www.raspberrypi.org/forums/viewtopic.php?f=56&t=100553). If you post there, you'll be reaching a wider audience of people (including myself) who can help you. Thanks for all of your comments!_

* * *

After [my last post](http://www.finnie.org/2015/02/14/ubuntu-14-04-trusty-tahr-on-the-raspberry-pi-2/), I went and ported Sjoerd's Raspberry Pi 2 Debian kernel patchset to Ubuntu's kernel package base (specifically 3.18.0-14.15). The result is an RPi2-compatible 3.18.7-based kernel which not only installs in Ubuntu, but has all the Ubuntu bells and whistles. I also re-ported flash-kernel based on Ubuntu's package, recompiled raspberrypi-firmware-nokernel, created a linux-meta-rpi2 package, and put it all in [a PPA](https://launchpad.net/~fo0bar/+archive/ubuntu/rpi2).

With that all done, I decided to go ahead and produce a base Ubuntu trusty image. It's 1.75GB uncompressed so you can put it on a 2GB or larger MicroSD card, and includes a full ubuntu-standard setup. Also included in the zip is a .bmap file; if you are writing the image in Linux you can use [bmap-tools package](http://packages.ubuntu.com/bmap-tools) to write only the non-zero bytes, saving some time. Otherwise it's the same procedure as other Raspberry Pi images.

(PS: If this image becomes popular, I should point out ahead of time: This is an unofficial image and is in no way endorsed by my employer, who happens to be the company who produces Ubuntu. This is a purely personal undertaking.)
