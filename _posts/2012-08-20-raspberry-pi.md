---
id: 2140
title: Raspberry Pi
date: 2012-08-20T15:34:44+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/?p=2140
permalink: /2012/08/20/raspberry-pi/
categories:
  - Uncategorized
tags:
  - planet:canonical
---
<div style="float: right; margin-left: 1em; margin-bottom: 1em;">
  <a href="http://www.flickr.com/photos/fo0bar/7811978806/" title="Raspberry Pi by Ryan Finnie, on Flickr"><img src="http://farm9.staticflickr.com/8432/7811978806_9ded4ec38b_m.jpg" width="240" height="180" alt="Raspberry Pi" /></a>
</div>

My [Raspberry Pi](http://www.raspberrypi.org/), the $35 barebones computer, arrived last week. I bought it for three primary reasons:

  1. It was cheap.
  2. It was interesting.
  3. It was cheap.

I ordered early on a Thursday morning, expecting 3-4 weeks before it shipped per [Element14](http://www.element14.com/raspberrypi)'s estimates. So I was quite surprised when I got a shipment notification later in the day. I'm guessing Element14 had a batch in, and was processing the backorders from the last few weeks, and somehow my order got mixed into that. So hey, yay instant gratification!

Shortly after that, [Adafruit's clear acrylic case](http://www.adafruit.com/products/859) became available again, so I ordered that as well. I was expecting a tighter fit, but instead it bounces around a bit within the case. It's still sturdy enough to do its job, and looks nice, though the top panel no longer has the etched Raspberry Pi logo like I've seen on previous photos.

[<img src="http://farm9.staticflickr.com/8299/7811979306_94b3199e4c.jpg" width="500" height="375" alt="Raspberry Pi" />](http://www.flickr.com/photos/fo0bar/7811979306/ "Raspberry Pi by Ryan Finnie, on Flickr")

I also ordered some cables and a powered USB hub from [Monoprice](http://www.monoprice.com/) (gotta love $5 next day shipping to California/Nevada), and a [high power iPad USB charger](http://eshop.macsales.com/item/Apple/MC359LLANB/) from OWC, but that turned out to be unnecessary. The [7-port Monoprice USB hub](http://www.monoprice.com/products/product.asp?c_id=103&cp_id=10307&cs_id=1030702&p_id=5328&seq=1&format=2) seems to provide enough power to power the Raspberry Pi itself, along with some accessories.

In the above setup, I've got it powering the Raspberry Pi (with HDMI output, plus 16GB SDHC card), a keyboard and mouse, a 4GB USB thumb drive, and my [Defcon 20 badge](http://www.flickr.com/photos/fo0bar/7653382862/). Not only does the Raspberry Pi boot, but I gave it a stress test for a few hours: Running a Quake 3 Arena 1920x1080 demo on loop (GPU), while simultaneously running Bonnie++ on the USB thumb drive (CPU and IO). It successfully did that for the entire test, so I'm confident in the power performance of the USB hub.

Now, I have no real plans for it; it'll probably sit, powered on in the corner of my office, for if I need to test something on an ARM system. But I do like the concept of its primary purpose. My first computer was a Vic 20 (predecessor to the Commodore 64), and then a Commodore 128 (backwards-compatible successor to the Commodore 64). I have fond memories of those computers, and they helped shape the course of the rest of my life. I like how the Raspberry Pi has both HDMI and Composite output, meaning it supports any TV or monitor made in the last 5 years or so, plus at least the previous 50 years of TVs. ([Remember RF adapters?](http://atariace.com/nintendo/accessories.php/item/34)) This really lowers the barrier of computer use for your average child.

However, there is one problem with the Raspberry Pi I really disagree with. Namely, it requires an operating system. [A 440MB operating system](http://www.raspberrypi.org/downloads), as well as the means to get it onto an SD card. An operating system that could easily be destroyed by curious fingers. This could be a huge barrier.

One of the things about my Vic 20 and Commodore 128 was I didn't have any cartridges or pre-bought software. Just the computer, a tape drive, and some CR-90 tapes. All value I got out of them was either by typing in BASIC programs from BYTE or Commodore magazines, or creating programs myself.

The Raspberry Pi really needs an onboard ROM with some sort of simple language on it, such as BBC BASIC, and the ability to save directly to SD cards. Upon boot, if the SD card contains a bootloader pointing to a full operating system, boot that, but if not, load the BASIC ROM.