---
author: Ryan Finnie
categories:
- Uncategorized
date: 2006-03-08 22:18:00
guid: http://www.finnie.org/2006/03/08/apt-get-purge-remove-gentoo/
id: 559
layout: post
lj_import_url:
- http://fo0bar.livejournal.com/144593.html
lj_itemid:
- '144593'
permalink: /2006/03/08/apt-get-purge-remove-gentoo/
title: apt-get --purge remove gentoo
---
Yesterday I blew away Gentoo from the laptop and installed Debian etch. This was done for several reasons:

1. I already manage over 100 Debian servers. At this point I know a lot more about Debian than I ever did about Gentoo, and it's just easier to manage at this point.

2. Gentoo's portage database has become bloated. VERY bloated. At this point, it takes well over 20 minutes to do an "emerge sync", and the database is about 400MB. I think it's now in Gentoo's best interest to move to an RPC-style datbase system. ("I need the latest version of foo." "OK, here is the description file for foo-1.0." "Hmm, looks like I need libbar to compile this, but my local configuration says only to use version 4.2 at the latest, if possible." "OK, here is the description file for libbar-4.1.5." etc)

3. No dependency checking for installed programs. For example, if I try to do "emerge unmerge xorg-x11", it will happily comply, despite the fact that many applications depend on the x11 libraries. Because of this, I imagine there was a lot of bloat lying around (can't exactly do "deborphan").

4. Frankly, I don't feel like sitting around for hours, waiting for 50 updates to compile anymore.

The conversion went suprisingly well...

* I am still using fluxbox. No fancy Gnome or KDE.
  
* The only program I've had to compile myself so far is ndiswrapper. It was in etch, but the required module for etch's current kernel (2.6.15-1-686) was not. wpa_supplicant worked out of the box, copying over my old configs from Gentoo.
  
* Debian's X configuration gave my laptop's native screen resolution (1280x768) as an option. Also, it properly detected the synaptics touchpad and mixed it in with USB's /dev/input/mice, without any intervention on my part. The only reason to edit xorg.conf so far was to disable touchpad tapping (I HATE tapping).
  
* ALSA sound was up and running in 10 minutes. The biggest problem was getting Firefox (flash plugin) to play sound. This was achieved by a wrapper called aoss (run "aoss firefox") that emulates /dev/dsp for that process. With that in place, everything can now play concurrently.
  
* xine works out of the box, but of course I had to manually download and install win32codecs to /usr/lib/win32.
  
* All of my gooey startup programs (xscreensaver, lineakd, gkrellm, etc) worked fine out of the box.
  
* Default bash settings are to assume UTF8 support everywhere, but my favorite terminal (aterm) doesn't support UTF8 yet. That was "solved" by setting LANG to C globally.
  
* Fonts have been the biggest issue so far, but I'm used to that with linux. Debian runs X at 100dpi by default, which just looks WEIRD when compared to equivalent displays in the 96dpi windows world (and of course those 72dpi OSX freaks). I "solved" that by forcing X to 96dpi. Firefox's font handling took some tweaking to get to the level I'm comfortable. And it only took 47 odd font packages to install, but I got it so I could read all that chinese and russian spam that I get in my gmail box. (I have my main personal email forwarded to gmail now, as it does excellent spam filtering.)
  
* cups was no problem, but it never is. I have a HP laserjet 1200, which is quite easy to set up.
  
* Some day I may port my old initrd cryptsetup stuff from gentoo so I can have an encrypted root partition again.
