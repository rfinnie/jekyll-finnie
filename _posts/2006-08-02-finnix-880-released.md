---
id: 603
title: Finnix 88.0 Released!
date: 2006-08-02T14:14:00+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/2006/08/02/finnix-880-released/
permalink: /2006/08/02/finnix-880-released/
lj_itemid:
  - "156927"
lj_import_url:
  - http://fo0bar.livejournal.com/156927.html
categories:
  - Finnix
---
Finnix is a small, self-contained, bootable Linux CD distribution for system administrators, based on Debian testing. Today marks the release of version 88.0 for the x86, PowerPC, and UML/Xen platforms.

Finnix 88.0 features Linux 2.6.17, a faster, more complete hardware autodetection routine, DMA mode enabled by default, Broadcom 43xx support, a DOS boot profile, and NTFS write support. Linux 2.6.17's new bcm43xx drive has been tested successfully on both G4 PowerBooks and x86 laptops with Broadcom cards, even with optional wpa_supplicant. FreeDOS ODIN, a 1.44MB image containing many DOS utilities may be booted by typing "dos" at the boot menu. The NTFS FUSE package, while present in Finnix 87.0, has been heavily tested, and seems to work rather well. Instead of mounting the normal way, simply type "ntfsmount /dev/hda1 /mnt/ntfsmount" to use the FUSE functionality.

  * Home page: <http://www.finnix.org/>
  * Download: <http://www.finnix.org/Download>
  * Release notes: [http://www.finnix.org/Finnix\_88.0\_release_notes](http://www.finnix.org/Finnix_88.0_release_notes)

_P.S. Many thanks to the [Oregon State University Open Source Lab](http://osuosl.org/) for providing primary Finnix release mirroring, after still-unresolved problems with SourceForge's mirroring system. Thanks OSL!_

_P.P.S. This announcement is being made a day early, as I'm confident I'll have internet access tomorrow evening, and I'd rather release a day early than a day late._
