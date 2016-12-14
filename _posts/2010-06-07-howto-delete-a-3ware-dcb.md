---
author: Ryan Finnie
date: 2010-06-07 13:51:18
guid: http://www.finnie.org/2010/06/07/howto-delete-a-3ware-dcb/
id: 1356
layout: post
permalink: /2010/06/07/howto-delete-a-3ware-dcb/
title: 'HOWTO: Delete a 3ware DCB'
---
I have a large enough infrastructure at work where we're often (once or twice per month) moving disks around, replacing failed disks on 3ware controllers, etc. One thing that often shows up is issues when an unwiped disk previously used in a 3ware controller is put into another 3ware controller. First, let's step back:

**Update (2010-06-08):** I added/updated some information based on what I've found analyzing a handful of DCBs in the last few days.

# 3ware DCB analysis

LSI 3ware (née AMCC 3ware, née 3ware) uses a proprietary disk layout that stores RAID information on the disks themselves. This has the advantage of being able to replace/upgrade a controller without having to reconfigure it. However, if you move a disk that was previously part of another array into a new machine without wiping it, it shows up to the new array as a failed existing array.

3ware stores this information on a disk in a section called the Disk Control Block (DCB). There are two versions of the DCB which I refer to as Old style and New style. On both Old and New, the DCB is stored at the end of the disk. I have discovered that the DCB appears to be 1024 LBAs long (1024 * 512 bytes per sector = 512KiB). Additionally, the Old style includes a copy of the DCB as the first 1024 LBAs of the disk. The actual disk data starts at LBA 0 on New style and LBA 1024 on Old style.

The Old and New style DCBs' 512KiB blocks are laid out differently, and I think I have found magic strings that can differentiate between them. On Old style, there is the string \x03\x00\xe9\x07 2044 bytes into the DCB, while New style has the string '3WareDCB' 2056 bytes into the DCB.

All 9000 series controllers I've seen so far seem to use New style, and until recently, all 8000 series controllers I had seen had Old style. I thought it was an 8000-versus-9000 thing, but I recently came across an 8006-2LP 2-disk RAID1 array that had a New style DCB. Updated firmware perhaps? Even odder about this array was that on all the New style DCBs I had seen until then, the actual disk data began at LBA 0, that is the beginning of the disk. However, on these disks, the disk data began at LBA 8. LBA 0-7 (4 KiB) contained something that sort of looked like a DCB (serial numbers are in there), but is much too short to be a proper DCB. This suggests that, at least with New style DCBs, the beginning data offset is stored somewhere within the DCB, but I have not yet been able to find it.

It should be noted that the DCBs on 8000 and 9000 series controllers are not compatible with each other. Plugging a disk previously used in an 8000 series controller (and not wiped) into a 9000 series controller will produce the dreaded UNCONV-DCB error. The disk is still usable as a (new) member of an existing array on the 9000, but it appears you cannot get rid of the UNCONV-DCB error until you take the disk out and manually wipe it. 9000 series are backwards and forwards compatible with each other, so you could take the disks from, say, a 9650SE and put them in a 9550SX or vice versa.

The previous paragraph's information comes from 3ware's docs and my personal experience, but that recent discovery of an 8006-2LP with a New style DCB throws a wrench into things. It could be that 3ware is actually referring to Old and New style DCBs when they say 8000 and 9000. If that's true, I may be able to put those New style 8006-2LP disks into a 9000 series controller. I'll have to try that sometime.

# Wiping the DCB

So, the important part to take away is if you want to reuse a disk that was previously in a 3ware controller on another 3ware controller as if it were a blank disk, it is best to wipe the disk's DCB areas first, to make sure the new controller doesn't recognize it. I'm currently writing a utility that can search a disk for DCBs, tell the difference between Old and New style DCBs, dump them to files, wipe them, etc. But for now, here's a short Linux shell script for wiping the DCB areas from a disk:

> <pre>DISK=sdz
LBAS=$(cat /sys/block/$DISK/size)
dd if=/dev/zero of=/dev/$DISK bs=512 count=1024
dd if=/dev/zero of=/dev/$DISK bs=512 seek=$(($LBAS-1024)) count=1024</pre>

Replace "sdz" with the device name (triple check; I'm not responsible for you hosing your boot drive). You can skip the first "dd" command if you are sure that the disk came from a disk with a New style DCB, but if you're "wiping" the disk with the intent of reformatting, it doesn't hurt to erase both the beginning and ending 1024 LBAs.
