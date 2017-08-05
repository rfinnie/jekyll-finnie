---
date: 2017-08-05 00:32:03-07:00
excerpt: Wherein Ryan turbocharges a pair of 12 year old obsolete computers.
  Wait, what?
excerpt_standalone: true
headline_image: /blog-media/2017/m2-to-44pin-ide.jpg
layout: post
title: M.2 SATA SSDs on Mac Mini G4s
---
It's 2017.
I have two PowerPC G4 Mac Minis which have been in service since they were released in 2005.
Long overdue for an upgrade.
No, I'm not going to replace them.
Instead, I'm giving them solid state drives.

My two G4 Mac Minis (`leo` and `inez` -- the home network uses Futurama characters for a naming scheme) were released in 2005, and for the most part, they have been running 24/7 since then, usually for [Finnix](https://www.finnix.org/) builds.
One of the nice things about the G4 Minis is they are very power efficient for the period -- about 12 watts at idle -- so I treat them as always-on servers.
`inez` is the more powerful of the two, with a 1.4GHz G4 and a 320GB 2.5" IDE HDD from an upgrade in 2010.
`leo` is a stock 1.2GHz G4 and an 80GB HDD.
Both have maxed out 1GB RAM.

But I've been thinking about their longevity lately, and not just because Debian and Ubuntu are dropping the 32-bit PowerPC architecture.
Namely, their hard drives.
IDE hard drives are not manufactured anymore, and they will eventually fail, being constantly spinning mechanical platters.
I checked `leo`'s original 12-year old hard drive SMART data, and its seek failures were so high, I'm surprised it's still running.
`inez`'s upgraded hard drive is "only" 7 years old and looks to be in good shape, but it will fail someday.

So what other options are available?
The drives are standard laptop-style 2.5" 44-pin IDE drives.
SSDs really didn't exist until the SATA era.
CompactFlash to IDE adapters are available, but most of the ones I've seen are "loose" (not in a form factor emulating a standard physical drive size), and besides, CompactFlash is basically obsolete too.
SD to IDE adapters exist and I've tried a few over the years, but compatibility tends to be terrible, and the interface is slow.

<img src="{{ "/blog-media/2017/m2-to-44pin-ide.jpg" | prepend: site.baseurl | prepend: site.url }}" alt="HP 15-f019dx" class="img-responsive img-rounded img-lg">

But the other day, I found something I didn't expect to exist: an M.2 SATA (up to 2280 length) to 44-pin IDE adapter, housed in a 2.5" enclosure.
And from what I read at the time, supposedly they worked well.
They're available from several outlets ([Amazon](https://www.amazon.com/gp/product/B06XC36V63/), [eBay](http://www.ebay.com/itm/SINTECH-M-2-NGFF-B-M-Key-SATA-SSD-to-44pin-2-5-IDE-adapter-card-with-case-/321532216470), [AliExpress](https://www.aliexpress.com/item/44-Pin-M-2-NGFF-SATA-SSD-to-2-5-IDE-SATA-SSD-Converter-SATA-Adapter/32796981722.html), etc), but all appear to be the same base manufacturer.
The difference is just price and how long you want to wait for them to arrive.
I ordered two from Amazon, as well as two of [literally the cheapest 128GB M.2 SATA drives available](https://www.amazon.com/gp/product/B01M9K0N8I/) ($52 each on sale at the time).

Migrating the hard drives was a bit of an ordeal.
The G4 Mac Minis have no room for expansion (the drive and a single stick of memory barely fit in the case as is).
My first idea was to use a 44-pin laptop IDE to 40-pin regular IDE adapter I had, install it in an old spare x86 machine I had, boot Finnix, recreate the partitions and sync from machine to machine.
But <kbd>mac-fdisk</kbd> is only available on PowerPC, so that was out.
I could have done the same with a G4 tower, but that's buried in a closet.
What I ended up doing was 44-pin to 40-pin, then 40-pin to USB.
The latter involved cannibalizing an old IDE CDROM to IDE enclosure, removing all but the PCB (because of the layout of the 44-pin to 40-pin adapter), but once I had that worked out, I attached the USB device to the Mac Mini.

Let me reiterate: at this point I was going from SATA to M.2 to 44-pin IDE to 40-pin IDE to USB.

The partitioning and syncing was relatively straightforward, but keep in mind for NewWorld PowerPCs, extra work is needed to make the bootstrap partition (#2) actually bootable, using <kbd>hattrib</kbd> to essentially do the following:

```
hmount /dev/sda2
hattrib -c UNIX -t tbxi :yaboot
hattrib -b :
humount
```

Once the new drives were physically swapped in after the transfers (which is physically a pain on G4 Mac Minis), I checked them out.
This is something I never expected to see from <kbd>smartctl</kbd> on a 12-year-old PowerPC:

```
Device Model:     ADATA SU800NS38
Rotation Rate:    Solid State Device
Form Factor:      M.2
ATA Version is:   ACS-3 (minor revision not indicated)
SATA Version is:  SATA 3.1, 3.0 Gb/s
```

I didn't do tests on `leo` with its nearly-failing drive before swapping it out, but I did do <kbd>bonnie++</kbd> on `inez` before.
Mind you, before the upgrade, its hard drive was a 320GB Western Digital Blue drive from 2010; essentially one of the last and most modern 2.5" drives produced.
And here's the before/after with literally the cheapest M.2 SATA drive I could buy new:

```
Version  1.97       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
inez(old)        2G           35513  17 16987  11            46172  13 157.3   6
Latency                        2113ms    1488ms             68443us    1952ms
inez(new)        2G           87089  40 41527  27           102046  27  4145 178
Latency                         836ms     229ms              7650us    8976us

Version  1.97       ------Sequential Create------ --------Random Create--------
                    -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
inez(old)        16 15257  77 +++++ +++ 20204  72 15349  75 +++++ +++ 19069  73
Latency               669us    1460us    1694us     691us     110us     671us
inez(new)        16 16910  85 +++++ +++ 22496  80 16591  82 +++++ +++ 21245  82
Latency               832us    1410us    1703us    9260us     120us     840us
```

I'm impressed.
I expected the IDE interface to be a large bottleneck, so I didn't expect those sorts of speed increases.

Side note: I bought two of the exact same drives.
ADATA SU800NS38 128GB, same firmware (Q0125D), similar serial numbers.
They both appear to perform about the same, but the raw disk sizes are different.
One is 128,035,676,160 bytes, one is slightly lower at 128,034,594,304 bytes.
Weird.
