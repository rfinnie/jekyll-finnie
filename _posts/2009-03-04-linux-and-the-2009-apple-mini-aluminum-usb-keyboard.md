---
id: 760
title: Linux and the 2009 Apple "mini" aluminum USB keyboard
date: 2009-03-04T23:25:00+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/testblog/wordpress/2009/03/04/linux-and-the-2009-apple-mini-aluminum-usb-keyboard/
permalink: /2009/03/04/linux-and-the-2009-apple-mini-aluminum-usb-keyboard/
lj_itemid:
  - "199913"
lj_import_url:
  - http://fo0bar.livejournal.com/199913.html
categories:
  - Uncategorized
---
It turns out they changed USB device ids on the new keyboard (not surprising, since it is physically different hardware), and since all Apple keyboards need special code to support the features such as Fn, the Linux kernel has to be updated to realize that the new device ids are Apple keyboards. [Here is the patch I submitted to LKML.](http://article.gmane.org/gmane.linux.kernel/802638)

Alternatively, if you just want to get the functionality temporarily: <tt>modprobe usbhid quirks=0x05ac:0x021d:0x00000800</tt>

What was surprising was that the USB device ids (0x021d, 0x021e, 0x021f) were numbered immediately before the regular aluminum keyboard (0x0220, 0x0221, 0x0222). Likewise, the model number (A1242) is immediately before the regular (A1243). Did Apple intend to release this keyboard at the same time as the other keyboards in August 2007? You heard it here first, kids.