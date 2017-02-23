---
categories:
- Finnix
date: 2005-11-21 21:33:00-08:00
layout: post
lj_import_url: http://fo0bar.livejournal.com/134323.html
lj_itemid: 134323
title: 'Announce (Woo!): Finnix 86.1 for x86/PowerPC/UML/Xen Released'
wp_id: 519
---
Finnix is a small, self-contained, bootable Linux CD distribution for system administrators, based on Debian testing. Today marks the release of version 86.1 for the x86, PowerPC, and UML/Xen platforms.

PowerPC Support: The largest change for version 86.1 has been the inclusion of a PowerPC port. Finnix for PowerPC (Finnix-PPC) is a 115MB ISO that functions identically to the main x86 Finnix distribution. Simply insert the CD and boot while holding down the "C" key. Finnix-PPC is well supported on G4 and NewWorld G3 hardware, including PowerMacs, PowerBooks, iBooks, iMacs and the Mac Mini. G5 support is present, but still experimental.

UML/Xen Support: Finnix 86.1 can be deployed as a guest image on User Mode Linux (UML) and Xen virtualization systems. UML/Xen administrators can go to https://www.finnix.org/uml.php for information on deploying Finnix in a UML or Xen environment. Additionally, the x86 Finnix CD includes a built-in UML demo. Once at the bash prompt, you can type "finnix create", and a UML guest session is created within the running CD session, using the same media as the CD session itself. Full networking is available in this running guest.

Other Changes: Finnix 86.1 includes Linux kernel 2.6.14.2, unionfs 1.1.1, and updated upstream packages. Many bugs have been fixed; most visibly, the CD will now reliably eject before system shutdown. For a full changelog, see the link below.

Home page: https://www.finnix.org/
  
SourceForge page: http://www.sourceforge.net/projects/finnix/
  
Download:
  
* x86: http://prdownloads.sourceforge.net/finnix/finnix-86.1.iso?download
  
* PowerPC: http://prdownloads.sourceforge.net/finnix/finnix-ppc-86.1.iso?download
  
ChangeLog: http://sourceforge.net/project/shownotes.php?group\_id=3892&release\_id=371261
  
Finnix for UML/Xen: https://www.finnix.org/uml.php
