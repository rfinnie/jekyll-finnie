---
date: 2012-03-09 22:46:42-08:00
layout: post
tags:
- planetcanonical
title: New home server
wp_id: 2032
---
<span style="float: right; margin-left: 1em;"><a href="https://www.flickr.com/photos/fo0bar/6822160034/" title="New home server (nibbler) by Ryan Finnie, on Flickr"><img src="https://farm8.staticflickr.com/7065/6822160034_71ed5d14cc_m.jpg" width="180" height="240" alt="New home server (nibbler)" /></a></span>It's been a long time since I've built a computer from scratch, and even longer since I've been excited about it. For the last few years, my home office network has been mostly kitbashed together from other computers, and when I've needed to buy new parts, it's usually been begrudgingly.

However, a few weeks ago, the stars all aligned. I was preparing to upgrade the RAM and drives on my colo box in San Jose after Ubuntu 12.04 is released, and went shopping. Hard drive prices are very high due to the flooding in Thailand last year, but RAM is dirt cheap. I was able to max out my colo server's RAM to 24GB (6x4GB) for $100. That got me thinking about my home office. At any given moment, 7 desktop computers were on: my main Ubuntu workstation, a Windows gaming machine, two G4 Mini Finnix dev machines (PPC), an C2D E7200 Finnix dev machine (x86), an Athlon64 3200+ Debian sid dev machine and an Athlon64 X2 4600+ router / miscellaneous server.

The desktops are used frequently, the G4s are insignificant (they consume 9w each), but the last three servers were all relatively old, very power hungry (and hot), and all perform tasks that need somewhat significant computing power, though usually never at the same time. And at idle, the three of them draw a combined 240w. So I decided now would be a good time to build a virtualization server and combine them. I considered upgrading one of them for the task, but decided it wouldn't work: The E7200, despite being relatively new, doesn't support VT. The 3200+ is nearing 10 years old (it was the first 64-bit consumer processor released) and definitely doesn't support VT. The 4600+ does, but it's pretty old, and the worst power offender.

No, it would be best to start from scratch. I decided to focus on an Intel Sandy Bridge processor, as much RAM as I could get, and a mid-range 6Gbps SATA drive for serving the VMs, with a focus on energy savings primarily and computing power secondarily. Here's what I ended up with:

  * [Intel Core i5-2400 CPU](http://www.newegg.com/Product/Product.aspx?Item=N82E16819115074) - $179.99
  * [MSI H67MA-E45 H67 chipset motherboard](http://www.newegg.com/Product/Product.aspx?Item=N82E16813130570) - $99.99
  * [G.SKILL 32GB RAM (4x8GB)](http://www.newegg.com/Product/Product.aspx?Item=N82E16820231486) - **$239.98!**
  * [Seagate Barracuda Green 2TB 6Gbps SATA drive](http://www.newegg.com/Product/Product.aspx?Item=N82E16822148681) - $129.99
  * [LG SATA DVD burner](http://www.newegg.com/Product/Product.aspx?Item=N82E16827136236) - $15.99
  * [Intel Gigabit PCIex card](http://www.newegg.com/Product/Product.aspx?Item=N82E16833106033) - $29.99
  * [Cooler Master Centurion 534 case](http://www.newegg.com/Product/Product.aspx?Item=N82E16811119106) - $59.99
  * [RAIDMAX 530W PSU](http://www.newegg.com/Product/Product.aspx?Item=N82E16817152028) - $39.99
  * [Rosewill 92mm CPU cooler](http://www.newegg.com/Product/Product.aspx?Item=N82E16835200056) - $17.99
  * [Hotplug front SATA bays (2)](http://www.newegg.com/Product/Product.aspx?Item=N82E16817998031) - $39.98
  * Total: $853.88

The parts arrived on Wednesday the 29th, I assembled it last weekend, and installed [Ubuntu precise Beta 1](https://wiki.ubuntu.com/PrecisePangolin/TechnicalOverview/Beta1) (AMD64 server). I was worried about building a Sandy Bridge system, the platform being relatively new, but nearly everything has worked perfectly so far. The CPU is well supported, the integrated graphics work, and all integrated components on the motherboard work fine. The only issue I've found so far is the system will not reliably power off after shutdown, but I can live with that. Heck, this is even the first machine I've owned where SATA hotplug works correctly.

The 6Gbps Seagate is a LUKS-crypted LVM disk holding the OS installations. The main install contains the routing / VPN / DHCP / etc services, as well as a QEMU/KVM host managed by libvirt. The Finnix dev and Debian sid dev machines have been transferred to this as VM guests. And Finnix build times have been reduced by nearly 2/3rds in the process.

Additionally, the host contains an existing 1.5TB WD Green SATA 3Gbps drive for shared media (ISOs, iTunes collection, movies, etc), and an existing 1.0TB WD Green SATA 3Gbps drive for backups of the other hosts. This drive is mounted in one of the front hotswap SATA sled trays for easy removal. (I learned this lesson late last year when I evacuated my home due to a nearby wildfire, and found that even quick-release case screws and toolless hard drive mounts were difficult to remove when you are short on time.) The other sled tray is simply for miscellaneous uses when needed.

So I replaced three servers, consuming 240w combined idle, with a beefy new server: powerful new CPU, 32GB RAM, and 3 hard drives totaling 4.5TB. I was expecting the idle power draw to be about 120w. But when I plugged it into my Kill-a-Watt, I found the power draw was really... 58w. Wow. And the CPU and case temperatures always hover around 28C. My first feeling was anger with myself; I should have spent the extra few dollars on the i5-2500 or i7-2600, but I was worried about power draw and heat dissipation. Oh well, the i5-2400 is still much faster than anything I've owned before.

I got all the functionality transferred over to the new server last weekend, but haven't done much with it for the last week. (I was pretty sick this week, missing four days of work.) This weekend I hope to make sure everything is in working order, and decommission the other servers.
