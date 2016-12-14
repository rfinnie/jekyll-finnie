---
id: 2426
title: 'Raspberry Pi 2 update - (unofficial) Ubuntu 14.04 image available'
date: 2015-02-16T22:41:02+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/2015/02/16/raspberry-pi-2-update-ubuntu-14-04-image-available/
permalink: /2015/02/16/raspberry-pi-2-update-ubuntu-14-04-image-available/
openid_comments:
  - 'a:8:{i:0;i:669892;i:1;i:669920;i:2;i:669947;i:3;i:669949;i:4;i:670000;i:5;i:670166;i:6;i:670235;i:7;i:670238;}'
categories:
  - Uncategorized
tags:
  - planet:canonical
---
[Download the lastest Ubuntu 14.04 Raspberry Pi 2 image](https://wiki.ubuntu.com/ARM/RaspberryPi)

If you downloaded an older image than the current one, you shouldn't need to reinstall, but be sure to review the changelog in the link above.

_Note that this blog post originally contained a bunch more information, which has been moved to a [dedicated page on wiki.ubuntu.com](https://wiki.ubuntu.com/ARM/RaspberryPi)._

_I've closed comments on this blog post. If you are looking for help, please see [this post on the raspberrypi.org forums](http://www.raspberrypi.org/forums/viewtopic.php?f=56&t=100553). If you post there, you'll be reaching a wider audience of people (including myself) who can help you. Thanks for all of your comments!_

* * *

After [my last post](http://www.finnie.org/2015/02/14/ubuntu-14-04-trusty-tahr-on-the-raspberry-pi-2/), I went and ported Sjoerd's Raspberry Pi 2 Debian kernel patchset to Ubuntu's kernel package base (specifically 3.18.0-14.15). The result is an RPi2-compatible 3.18.7-based kernel which not only installs in Ubuntu, but has all the Ubuntu bells and whistles. I also re-ported flash-kernel based on Ubuntu's package, recompiled raspberrypi-firmware-nokernel, created a linux-meta-rpi2 package, and put it all in [a PPA](https://launchpad.net/~fo0bar/+archive/ubuntu/rpi2).

With that all done, I decided to go ahead and produce a base Ubuntu trusty image. It's 1.75GB uncompressed so you can put it on a 2GB or larger MicroSD card, and includes a full ubuntu-standard setup. Also included in the zip is a .bmap file; if you are writing the image in Linux you can use [bmap-tools package](http://packages.ubuntu.com/bmap-tools) to write only the non-zero bytes, saving some time. Otherwise it's the same procedure as other Raspberry Pi images.

(PS: If this image becomes popular, I should point out ahead of time: This is an unofficial image and is in no way endorsed by my employer, who happens to be the company who produces Ubuntu. This is a purely personal undertaking.)
