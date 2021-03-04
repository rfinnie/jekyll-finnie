---
date: 2021-03-03 17:52:55-08:00
excerpt: Wherein Ryan probably owns the only SSD-based Google Mini search appliance
  in existence
excerpt_standalone: true
headline_image: /blog-media/2021/derust-pyramid.jpg
layout: post
title: (Spinning) Rust begone!
---
<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2021/derust-pyramid.jpg" alt="33 hard drives arranged in a pyramid pattern" class="img-responsive img-rounded img-lg">

You could say I have a few computers... 63 as of this writing (I made a
spreadsheet), though about half of those are SBCs (single board
computers; Raspberry Pis and similar). However, many of the other
computers are old, and include mechanical hard drives in various states
of failure. About a year ago, I began a quest to eliminate as many
mechanical hard drives from my collection as possible.

## 2.5" SATA

This one's simple, just replace the 2.5" SATA HDD with a 2.5" SATA SSD.

## 3.5" SATA

<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2021/derust-3.5-sata-underside.jpg" alt="3D printed 2.5 inch to 3.5 inch drive adapter with case-specific standoffs and an SSD installed" class="img-responsive img-rounded img-md pull-right">Almost as simple as 2.5" SATA, just replace with a 2.5" SATA SSD and a 2.5" to 3.5" adapter bracket. However, it took me awhile to find the ideal bracket, [a 3D printed universal-ish adapter](https://www.thingiverse.com/thing:29628) (specifically the "minimalist sunk screwholes" variant). It can mount the underside of an SSD in several positions, and has 3.5" holes for side or underside mounting.

Note that while the SATA/power ports are close to a normal 3.5" drive's location, it's not exact. For a situation where you need to plug into a backplane (like a server or NAS), you'll need a [caddy adapter which re-routes the ports](https://www.amazon.com/General-Drive-HDD-Adapter-CADDY/dp/B00F3QFKNS/).
<div class="clearfix"></div>

## 3.5" IDE

<div class="pull-right"><img src="{{ site.url }}{{ site.baseurl }}/blog-media/2021/derust-sd-slot.jpg" alt="SD to IDE adapter in a 3D printed PCI/ISA bracket" class="img-responsive img-rounded img-md"><img src="{{ site.url }}{{ site.baseurl }}/blog-media/2021/derust-sd-3.5.jpg" alt="SD to IDE adapter in a 3D printed 3.5 inch mount" class="img-responsive img-rounded img-md"></div>These became the majority of my conversions. Since we're talking about older IDE devices, the speed of a modern SSD isn't needed, so [SD to IDE adapters](https://www.amazon.com/KOOBOOK-1Pcs-40Pin-Adapter-Drive/dp/B07YFPX7JB/) work extremely well. (The linked product is only an example; it's a common design sold by many sellers under similar names.) I've had success across a range of devices, from a 486 desktop to a Sun Blade 100 workstation to a Power Mac G4 desktop.

The preferred mounting option is in a PCI / ISA slot, so you can remove the SD card and mount it on a more modern computer if needed. I designed [a reinforced bracket for this purpose](https://www.thingiverse.com/thing:4686963).

Alternatively, you can mount it in a 3.5" bay, either as an external 3.5" device (i.e. a floppy bay) or internal, if all else fails. Again, [I have designed a 3D-printed mount for these situations](https://www.thingiverse.com/thing:4572090). The same mount can either be used internally or externally, with side or underside 3.5" mounting. Note that the adapter's IDE and power ports are nowhere near a normal hard drive's positioning, and the top-mounted Molex port is rarely in a convenient location, so you'll also want to get a short 40-pin extension ribbon cable, and a short floppy power to Molex adapter.

## 3.5" IDE (for stubborn computers)

The only computer I found which doesn't like the SD to IDE adapter is the Power Mac G3 Blue & White. Mac OS 8.6 sees the drive, but would somehow encounter I/O timeouts any time it tried to use it. For this machine, I went with a CompactFlash card and [a CF to IDE adapter](https://www.amazon.com/Ximimark-Compact-Bootable-Adapter-Converter/dp/B07LBLXDZM/). CF is a subset of IDE, so you want the adapter to be completely passive. This worked, but as CF cards are getting harder (and more expensive) to find, I wanted to use this as a last resort.
<div class="clearfix"></div>

## 2.5" IDE

<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2021/derust-m2-2.5.jpg" alt="M.2 to 2.5 inch IDE adapter with M.2 drive installed" class="img-responsive img-rounded img-md pull-right">This one's a bit weird. For older laptops and machines like the G4 Mac Mini, it needs to be the same form factor as a 2.5" IDE drive, so we'll want to get... M.2 drives. Okay, SATA, not NVMe, but it still feels odd buying a brand new gumstick-sized card for a 15+ year old device. The secret is [this M.2 SATA to 2.5" IDE converter](https://www.amazon.com/gp/product/B07Z67GX6W/) (again, sold by multiple sellers); beyond that, you can pick any cheap M.2 SATA 2242 drive, though I recommend 120GB drives as the target device is probably old enough to be affected by the 48-bit LBA limit of 137GB.

## SCSI

I have several machines which have both SCSI and IDE interfaces, and for those I just used the methods above for IDE. But I do have one SCSI-only machine: the SGI Challenge S, a server variant of the SGI Indy. For this, there is the [SCSI2SD v6](http://www.codesrc.com/mediawiki/index.php/SCSI2SD). Until now, the converters/adapters have all been in the $10 to $20 range, but the SCSI2SD is significantly more expensive at about $100. But it's full-featured, and has options for just about any classic computer situation. It can divide up an SD card to emulate multiple SCSI devices, even CD/tape drives, and has software-configurable termination options. I love it, but at its price, I'm glad I only needed it once.

Again, you'll probably need a bracket to physically mount it within the computer, but I found [someone else's SCSI2SD universal mount](https://www.thingiverse.com/thing:3067337) to work fine with the Challenge S without modification.
<div class="clearfix"></div>

## Related: floppy drives

<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2021/derust-gotek.jpg" alt="Gotek floppy emulator with OLED mod and 3D printed case, mounted in a Packard Bell desktop" class="img-responsive img-rounded img-md pull-right">Floppy drives and disks also fail, and for this I recommend the [Gotek SFR1M44-U100](https://www.amazon.com/gp/product/B0762NCHC6/), which lets you use a USB thumb drive to emulate floppy disks. There are many mods you can do to this drive, but at the very least, I recommend replacing the stock firmware, which requires a proprietary utility to write images to the USB drive. Replace the firmware with [FlashFloppy](https://github.com/keirf/FlashFloppy), which has many more emulation options than the stock firmware, and also lets you directly drop image files onto the USB drive.

## What's left?

Surely I didn't completely eliminate spinning hard drives from my home, did I? Something I found was the closer I got to zero, the more the remaining ones stood out, and the stronger the desire to address them.

  - [Some Cheap Laptop from Best Buy](https://www.finnie.org/2015/02/05/review-some-cheap-laptop-from-best-buy/) was a laptop I used for a few months back in 2015. I consulted the repair manual for the laptop, and getting to the 500GB HDD would have been a monumental task. The laptop itself was not useful to me, nor did I have any sentimental attachment (beyond the joke review I made 6 years ago), so I solved the problem by reinstalling Windows 8.1 on it and giving it away.
  - My primary home server has 28TB of raw HDDs, in the form of 7x 4TB drives. It would be prohibitively expensive to replace it all with SSDs. One drive is external backup and one is a Purple drive for security camera recordings, and the other 5 are in a RAID 6 setup, backed by a 500GB SSD bcache, so I'm not worried about reliability or performance.
  - My Windows gaming machine has 3 tiers of storage: 1TB of boot NVMe, 2TB of SATA SSD, and a 6TB HDD. Again, I'm not concerned.
  - My remote colocation server has two primary boot 250GB SATA SSDs, and two WD RE4 2 TB "enterprise" HDDs, each in RAID 1. I'll likely do pure solid-state with my next colocation server, but considering the current server is only 3 years old and the one before that lasted nearly a decade, it's probably not going to be for quite awhile.
