---
author: Ryan Finnie
date: 2015-02-05 17:02:14
guid: http://www.finnie.org/2015/02/05/review-some-cheap-laptop-from-best-buy/
id: 2411
layout: post
permalink: /2015/02/05/review-some-cheap-laptop-from-best-buy/
title: 'Review: Some Cheap Laptop from Best Buy'
---
Last week I sold my primary laptop, a ThinkPad X220. After nearly three years of constant use, it was still in excellent shape, and was still powerful enough for daily use. Frankly, I was amazed how well it held up. None of the keycaps were worn off, everything was still solid, and none of the plastic corners had chipped. A "normal" laptop would survive less than a year of use with me, and a ThinkPad would get maybe two years. For a laptop to survive three years of me was a true testament to its build quality.

There were several reasons I sold it: Primarily, my company buys me a new laptop for every 3 years of service, and my 3 year anniversary was in January. I'm expecting to buy the ThinkPad X250 as soon as it's released, but that has not yet happened. I would have kept the X220 as a backup laptop, except a friend was looking for a used X220 specifically, so I sold it to him.

I figured I could temporarily fall back on my 9 year old ThinkPad T60 (my first ThinkPad), which had been collecting dust. But after a day or so of use, I realized it is no longer suitable for even temporary use. Firefox would choke on the combination of 1GB of RAM and an ancient graphics card. And the only other laptop I owned was even more ancient, a Compaq Presario V2000z (AMD Turion CPU, 512MB RAM, 10 years old).

I needed a usable laptop to get me through the next few weeks, and something for future occasional use. I did some quick research, and settled on Some Cheap Laptop from Best Buy. Some Cheap Laptop from Best Buy was the cheapest available laptop which I could pick up locally and contained a bare minimum of personal requirements:

  * Intel Core i3 Haswell or higher (most of the low-end models were AMD E series, which appear to be equivalent to Intel Atoms)
  * No Chromebooks
  * 4GB or more RAM
  * Under 8lbs

Some Cheap Laptop from Best Buy satisfied these requirements:

  * CPU: Intel Core i3-4030U
  * Memory: 6GB RAM
  * Display: 15.6" 1366x768 TN
  * Storage: 500GB 5400RPM HDD
  * Optical: DVD-RW drive
  * Battery: if you could call it that
  * Weight: 5.05lbs
  * Official designation: HP 15-f019dx
  * Price: $329.99

[<img src="/blog-media/2015/02/quiwoo-760-1024x768.jpg" alt="HP 15-f019dx" width="788" height="591" class="aligncenter size-large wp-image-2415" srcset="/blog-media/2015/02/quiwoo-760-1024x768.jpg 1024w, /blog-media/2015/02/quiwoo-760-300x225.jpg 300w, /blog-media/2015/02/quiwoo-760-788x591.jpg 788w" sizes="(max-width: 788px) 100vw, 788px" />](/blog-media/2015/02/quiwoo-760.jpg)

Some Cheap Laptop from Best Buy's 15.6" display is massive by my 12.5" tastes, and I don't care for the numpad, but since it's "only" 5.05lbs, it's managable. (By comparison, the X250 is going to be approximately 3lbs.) The entire surface is a nice matte black, and the touchpad feels comfortable, as far as touchpads go. (I miss the TrackPoint already.) My biggest complaint with the touchpad is it's too large, and my left palm tends to slightly rest on the left side of it while using my right finger to natigate, resulting in scrolling instead of mouse movement. The island-style keyboard is usable, but I can't say how well it is since I've never yet owned a keyboard with island keys.

The screen is bright (a little too bright for my taste, but the brightness can be dialed down), but suffers from massive color inversion at angles. The color balance itself was way too blue ("Showroom Syndrome", as is way too common in the marketplace), but was easily corrected with a new ICC profile via my Spyder 2.

Some Cheap Laptop from Best Buy came pre-installed with Windows 8.1. Like all new computers these days, it did not come with any physical restore media, but thankfully the program to burn restore DVDs was easy. (I don't plan on ever needing Windows on this laptop, but it's always nice to have restore media just in case.) I created the restore media, then nuked the hard drive and installed Ubuntu 14.10 Utopic Unicorn. The installation went without problem, and the installed environment has been completely fine.

Like almost all new laptops these days, the top row of keys default to the media keys (brightness/volume control, etc), with the Fn key toggling to F1 through F12. There is an option in the BIOS to flip the logic, which I did. Unfortunately, the Insert and Print Screen functions also share the same button, with Print Screen being the default, and Insert requiring Fn. The BIOS option does not flip this. Since I use Shift-Insert a lot, I flipped them at the udev HWDB level, by putting this in `/etc/udev/hwdb.d/60-keyboard.hwdb`:

<pre>keyboard:dmi:bvn*:bvr*:bd*:svnHewlett-Packard:pnHP15NotebookPC:*
 KEYBOARD_KEY_90=previoussong
 KEYBOARD_KEY_99=nextsong
 KEYBOARD_KEY_a0=mute
 KEYBOARD_KEY_a2=playpause
 KEYBOARD_KEY_ae=volumedown
 KEYBOARD_KEY_b0=volumeup
 KEYBOARD_KEY_b7=insert # swapped with d2
 KEYBOARD_KEY_c5=pause
 KEYBOARD_KEY_d2=sysrq # swapped with b7
</pre>

and running:

<pre># udevadm hwdb --update
# udevadm trigger
</pre>

Overall, performance is decent. Web browsing is nice and fast, and I can keep my usual 50 or so terminals open without a problem. One interesting thing is while the i3-4030U CPU is slower than the i5-2540M on the X220, the GPU is actually faster. Minecraft actually runs marginally better on this laptop!

In conclusion, if you are a price-conscious person waiting for the ThinkPad X250 to be released, but have already sold your X220 and need a temporary everyday laptop quickly, I'd recommend Some Cheap Laptop from Best Buy. Except by the time you've read this, it's probably already been discontinued and replaced with Another Cheap Laptop from Best Buy.
