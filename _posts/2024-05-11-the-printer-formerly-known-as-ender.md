---
date: 2024-05-11 10:56:00-07:00
excerpt: Three years ago, I bought an Ender 3 V2, and have so heavily modified it
  that it really can't be called an Ender 3 anymore. Let's call it the Filimanator
  9,000,000 instead.
excerpt_standalone: true
headline_image: /blog-media/2024/ender-3-v2-modified.jpg
layout: post
title: The printer formerly known as Ender
---
<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2024/ender-3-v2-modified.jpg" alt="Ender 3 V2 3D printer, heavily modified" class="img-responsive img-rounded img-lg">

[Five years ago]({{ site.url }}{{ site.baseurl }}/2019/05/04/monoprice-maker-select-plus-3d-printer-mods/), I wrote about the 3D printer I had recently bought, a Monoprice Maker Select Plus (Wanhao Duplicator i3 Plus), and the various mods I had done to it. I had discovered that when it comes to 3D printing, for me the journey was much more fun than the destination, and while I often use 3D printers for "practical" purposes, hacking on the printer itself is the most fun.

In that post, I said:

> 3D printers are like cats: people who have more than zero usually have more than one. Some even have their houses overrun by them.

Two years after that, I bought another printer, a Creality Ender 3 V2.  In the same spirit, it's a very hackable printer, to the extent that I'm not even sure I should call it an Ender 3 anymore.  Most of the components have been modified or replaced in the 3 years I've had it, with the latest being a direct extrusion conversion. Everything orange you see in that picture is a 3D printed mod.

Admittedly, a lot of it is change for change sake, but a lot is also for performance, quality and reliability reasons. For a hobbyist printer, it works amazingly, and produces nearly perfect [Benchys](https://www.3dbenchy.com/), though of course the speed isn't as great as a hyper-modern printer like a Bambu.

While I am perpetually on the edge of building a full kit from scratch like a Voron, my time with the Ender 3 V2 has been very enjoyable. Below is an account of all the changes I've made to the printer since I bought it 3 years ago.

---

The newest addition (within the last few weeks) is a direct drive extruder, the [Creality Sprite SE](https://www.amazon.com/gp/product/B0C7QC7S3D). (Not to be confused with the dozen other variants of the Creality Sprite. Creality has a tendency to do that with everything it makes.) This replaced the Bowden tube system; the only other accessory to note is a [simple cable routing channel](https://www.tinkercad.com/things/b7OohBxQFKU-e3v2-direct-drive-top-cable-mount) I designed so the cable bundle didn't get pinched on the sides when moving around.

Most of the hotend assembly has been upgraded. While many people tend to upgrade the hotend itself, I left it alone and played with most of the stuff surrounding it.  [The fan assembly](https://www.printables.com/model/80823-yet-another-ender-3-v2-fan-assembly-remix-satsana-) is a complete replacement, supporting a better [hotend cooling fan](https://www.amazon.com/gp/product/B0757RPCN9) and [part cooling fan](https://www.amazon.com/gp/product/B0755BY9RH).  A [BLTouch](https://www.amazon.com/gp/product/B08MD45N9H) automatic bed leveler is also added. (That link now goes to Creality's CRTouch alternative, but when I bought it, it was a genuine Antclabs BLTouch, back when they were partnering directly with Creality.)

The [LCD panel](https://www.thingiverse.com/thing:4753473) has been [modified](https://www.tinkercad.com/things/cU3Gijv3t8R-ender-3-v2-lcd-panel-modified), changing it from portrait on the side to landscape in front, with a new [control knob](https://www.thingiverse.com/thing:463660). The UI has changed from CrealityUI to MarlinUI, and because of this (and the many other mods), I'm running standard [Marlin](https://github.com/MarlinFirmware/Marlin), with [my own custom configuration](https://github.com/rfinnie/marlin-personal).

The Z axis has been upgraded to [dual screw with dual motors](https://www.amazon.com/Official-Creality-Ender-Z-axis-Stepper/dp/B07VJG4ZCG), though they are both controlled by the same stepper driver, so it's more about stability than independent calibration. In addition, the Z axis [shaft couplers](https://www.amazon.com/gp/product/B07RMZCLZ3) have been upgraded to reduce horizontal shaft movement.

<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2024/ender-3-v2-modified-underside.jpg" alt="Ender 3 V2 3D printer, heavily modified, underside view" class="img-responsive img-rounded img-md pull-right">The entire underside has been pretty extensively reworked, as the stock Ender 3 V2 was pretty starved for airflow. The covers for the [mainboard](https://www.thingiverse.com/thing:4653994) and [PSU](https://www.thingiverse.com/thing:4713952) [area](https://www.thingiverse.com/thing:4831917) have been replaced with prints which allow for [quiet 80mm fans](https://www.amazon.com/gp/product/B00IOIJ4AC), but as they are 12v fans and the printer is a 24v system, an [LM2596 buck converter](https://www.amazon.com/Zixtec-LM2596-Converter-Module-1-25V-30V/dp/B07VVXF7YX) is needed. To support all of this, the printer needs to be raised up a few inches, so [these feet](https://www.thingiverse.com/thing:4721832) are used (which even incorporate the original printer's rubber feet).

Last year, I was having issues with Y axis shifting.  The problem ultimately ended up being the motor itself was losing steps, so I needed to replace [the motor](https://www.amazon.com/gp/product/B091D37BM2) and [pulley](https://www.amazon.com/gp/product/B088WB8D7W) (since the factory motor includes the pulley permanently attached). However, there was also the the concern it may have been the stepper driver on the mainboard itself, and the drivers are not modular. While this ended up not being the case, I ended up buying and installing a replacement [V4.2.7 mainboard](https://www.amazon.com/gp/product/B09NMJMPN1), which is a direct replacement for the stock V4.2.2 board, with some very minor improvements.

Not much has been done to the bed itself, with the only addition being [upgraded springs](https://www.amazon.com/gp/product/B07FY47BX7), though the BLTouch makes manual bed leveling unnecessary (you're good if you can keep it within 0.5mm of level or so). However, I did simply remove the rear left screw from the bed, making it a 3-point system, which helps reduce flex within the bed itself.

The [filament spool holder](https://www.thingiverse.com/thing:3209211) has been upgraded to add [skateboard bearings](https://www.amazon.com/Sackorange-Skateboard-Bearings-Miniature-Bearings%EF%BC%88Pack/dp/B07216D1SZ). And finally, a [simple filament feed arm](https://www.tinkercad.com/things/iMyXIBttRNl-ender-3-v2-filament-feed-arm), though this isn't needed as much today, since I replaced the Bowden system with direct extrusion.
