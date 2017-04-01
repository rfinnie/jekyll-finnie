---
date: 2017-04-01 12:02:53-07:00
excerpt: While native Linux md RAID 10 works well, its main problem is it doesn't
  indicate what each individual disk is doing. Its sub-members can be deduced by examining
  the contents of the raw disks themselves.
layout: post
title: Linux md RAID 10 disk layout, updated
---
[Back in 2012](https://www.finnie.org/2012/11/04/linux-md-raid-10-disk-layout/), I wrote about how to identify the individual components of a Linux md RAID 10 array.
mdraid recently (within the last decade, that is) allows for native RAID 10 arrays, whereas before the normal method would have been to create two sets of RAID 1 arrays, then combine those arrays into a RAID 0 array.

While native RAID 10 has worked well over the last 5 years, its main problem is it doesn't indicate what each individual disk is doing.
Are sdc and sdd members of the same RAID 1 pair, etc.
In 2012, I figured this out by doing tests using loopback devices and observing their behavior when failed/removed/etc, but for a mature, in-use setup, I realized there is a much easier way to figure this out: look at the contents of each disk.
Skip about 1 GiB into each of the disks, read 1 MiB, and hash them.

<pre># cat /proc/mdstat
Personalities : [raid10] [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4]
md127 : active raid10 sdf1[3] sdd1[2] sde1[1] sdc1[4]
      3900437504 blocks super 1.2 512K chunks 2 near-copies [4/4] [UUUU]</pre>

<pre># for i in sd{c,d,e,f}; do
  dd if=/dev/$i of=/tmp/check_$i bs=1M skip=1024 count=1;
  done</pre>

<pre># sha256sum /tmp/check_sd{c,d,e,f}
d6818ca0955af1ed2023c0f30fa257eff6ce28e01960ff2720e58c78403c5775  /tmp/check_sdc
25cd21e90dae3f666362e724d2d3af66830817352d468cb67d2bf00d615ef9a0  /tmp/check_sdd
d6818ca0955af1ed2023c0f30fa257eff6ce28e01960ff2720e58c78403c5775  /tmp/check_sde
25cd21e90dae3f666362e724d2d3af66830817352d468cb67d2bf00d615ef9a0  /tmp/check_sdf</pre>

There we go.
sdc and sde are part of the first RAID 1 pair, sdd and sdf are part of the second RAID 1 pair, and both pairs are combined into RAID 0.
I've updated my table as so:

<pre>|-------------------------------------------|
|                   RAID0                   |
|-------------------------------------------|
|        RAID1        |        RAID1        |
|---------------------|---------------------|
| 4.0TB 1A | 2.0TB 1B | 2.0TB 2A | 2.0TB 2B |
| WDH0YBVV | 5YD0VFHK | 5YD0ZQTN | 5YD0P0JF |
| sdc      | sde      | sdd      | sdf      |
|-------------------------------------------|</pre>

---

The reason I re-visited this was, well, this array is approaching 5 years of service, with consumer drives.
Late last year, one of the 2TB drives indicated SMART predictive failure, and I replaced it with a 4TB drive (albeit with a first partition the same size as the existing 2TB drives).
The plan now is to replace the remaining drives over the year with 4TB drives, using different batches (and different manufacturers).
All four drives were originally bought at the same time, and have a higher chance of having more than one failure at a time, which could be disastrous.

I've got a 4TB WD drive on order (all the current drives are Seagate), and ideally wanted to place it in the RAID 1 array which didn't contain the drive I replaced last year.
That way, each of the RAID 1 pairs will contain both an old and a new drive.

Once all four drives are replaced with 4TB drives, I'll be left with an identical array set as before, but with 2TB per drive completely unused.
At that point, I can partition the remaining space in each drive and combine them into a second RAID 10 array, then add it as a second physical volume on the LVM setup this is all a part of.
