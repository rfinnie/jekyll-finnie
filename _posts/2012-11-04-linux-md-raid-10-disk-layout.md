---
id: 2175
title: Linux md RAID 10 disk layout
date: 2012-11-04T19:23:55+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/?p=2175
permalink: /2012/11/04/linux-md-raid-10-disk-layout/
openid_comments:
  - 'a:1:{i:0;i:397049;}'
categories:
  - Uncategorized
tags:
  - planet:canonical
---
I'm working on re-doing my home router / VM server to provide better IO. The goal is to have an SSD as the boot drive, and 4 2TB disks in RAID 10 (4TB usable total) for the VMs. I'll be using md RAID for building it, however I want to be particular about where the drives are physically, for a few reasons:

  * The RAID drives are all SATA 6Gbps drives, but the server's motherboard only has 2 SATA 6Gbps ports. I've got a 2-port PCIe SATA 6Gbps controller on the way<sup>[0]</sup>.
  * I want to place the two drives in each base RAID 1 set on different controllers: one on the motherboard controller, one on the PCIe card controller. This may provide extra performance, but more importantly, having each disk on a different controller protects the array as a whole in case of a controller failure.

Linux md RAID 10 is a relatively new mode. In the past, you would manually create multiple RAID 1 arrays, then combine them in RAID 0, thereby knowing where the disks were placed, since you were doing it yourself. The mdadm RAID 10 method is easier, but I found there is literally no documentation on what drives it uses for the underlying RAID 1 arrays. Using loopback devices and some trial and error, I figured out how the arrays are assembled.

In a nutshell, the underlying RAID 1 arrays are paired two at a time, in order given during creation. If you were to do this:

<pre># mdadm --create --verbose /dev/md0 --level=10 --raid-devices=4 /dev/sd{a,b,c,d}1
</pre>

sda1 and sdb1 form one RAID 1 array, and sdc1 and sdd1 form another:

<pre>|---------------------------|
|           RAID0           |
|---------------------------|
|    RAID1    |    RAID1    |
|-------------|-------------|
| sda1 | sdb1 | sdc1 | sdd1 |
|---------------------------|
</pre>

One other thing to mention is what happens when multiple drives are lost. Say in this case, both sda1 and sdd1 are lost, resulting in a degraded but functional array:

<pre># mdadm --fail /dev/md0 /dev/sda1
# mdadm --remove /dev/md0 /dev/sda1
# mdadm --fail /dev/md0 /dev/sdd1
# mdadm --remove /dev/md0 /dev/sdd1
|---------------------------|
|           RAID0           |
|---------------------------|
|  RAID1 (D)  |  RAID1 (D)  |
|-------------|-------------|
|      | sdb1 | sdc1 |      |
|---------------------------|
</pre>

If you were to replace sdd and add it back first, you might think it would go in the second RAID 1 array. But no, it takes the first available degraded slot:

<pre># mdadm --add /dev/md0 /dev/sdd1
|---------------------------|
|           RAID0           |
|---------------------------|
|    RAID1    |  RAID1 (D)  |
|-------------|-------------|
| sdd1 | sdb1 | sdc1 |      |
|---------------------------|
</pre>

So be careful in this situation, if you care about where the devices are physically laid out.

<sup>[0]</sup> Half of the order actually arrived Friday, including the SSD and a 4-port PCIe 4x SATA 6Gbps controller. The idea was to place two of the RAID drives on the motherboard SATA 6Gbps controller, and two on the new controller, plus the boot SSD (which is also SATA 6Gbps). My system has 3 PCIe 1x ports and a PCIe 16x port. The 4x card was supposed to go in the 16x port, but I learned after failure that many motherboards do not like non-video cards in the primary 16x port. Oh well. The boot SSD will now go on one of the motherboard SATA 3Gbps ports, and I've got an order in for a 2-port PCIe 1x SATA 6Gbps controller.
