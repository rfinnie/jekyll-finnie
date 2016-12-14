---
author: Ryan Finnie
categories:
- Finnix
date: 2005-11-04 10:53:00
guid: http://www.finnie.org/2005/11/04/calling-all-mac-geeks/
id: 513
layout: post
lj_import_url:
- http://fo0bar.livejournal.com/132819.html
lj_itemid:
- '132819'
permalink: /2005/11/04/calling-all-mac-geeks/
title: Calling all Mac geeks!
---
Finnix-PPC development is coming along nicely. It's "stable", but since I don't have 100 different ppc machines lying around like I do for x86, I'm turning to livejournal for hardware testing before I "publicly" release it. So if you have a NewWorld[0x01] Mac (ESPECIALLY a G5):

<!--more-->1) Download and burn 

[finnix-ppc-dev21.iso](http://coma.colobox.com/finnix/finnix-ppc-dev21.iso) (116MB, login is finnix/2000, MD5 479b8f49d12148852520212a4cc66647)
  
2) Inset disc, reboot while holding down "c".
  
3a) For G3/G4 machines, simply hit Enter at the boot prompt.
  
3b) For G5 machines, type "finnix64" at the boot prompt.
  
4) Note any errors or problems (didn't find wired network, etc, etc). If the kernel boot displays warnings about pmac_zilog or "Cannot allocate resource region", you can safely ignore them.
  
5) Make sure you are wired in; you will need internet access in the next step, and Airport Extreme cards are not supported in linux yet.
  
6) Run "finnix-hwsubmit", which will gather info about your hardware, let you review the info to be sent, then send it to the Finnix server. Follow the instructions. [Here](http://www.finnix.org/submits/ppc/1131131838-3492276741-220483593.gz) is an example of a submitted report. If you could note your LJ username in the "notes" section of the program, I would appreciate it :)
  
7) Play with Finnix or something.

If something stupid happens like it doesn't detect your keyboard or wired network, and hence you can't get finnix-hwsubmit to submit the report, please reply here.

I have personally tested Finnix-PPC on the following machines:
  
* 400MHz Graphite G4, 1GB (original dev machine)
  
* 400MHz FW G3 iMac, 128MB
  
* 1GHz MDD G4 Windtunnel, 1GB
  
* 1GHz 12" G4 iBook, 512MB
  
* 1.25GHz Mac Mini G4, 256MB

With the exception of the grey screen bug and the lack of airport extreme, everything has worked like a charm on those 4 machines, even the built-in airport classic on the Graphite G4. If you have ANY NewWorld hardware, I would appreciate you testing this.

User-submitted machines, all mentioned have worked great so far:
  
* 1.5GHz 15" Aluminum PowerBook G4, 768MB
  
* 1.66GHz 15" PowerBook G4, 2GB
  
* 1.4GHz Mac Mini G4, 512MB

Update: Looks like G5 hardware will NOT work. You can still try if you want to :)
  
Update 2: I think I now know what's causing the grey screen problem. We'll see...
  
Update 3: Yep, all it took was adding 1 line to the openfirmware boot script. Goodbye grey screen!
  
Update 4: New version posted (dev14).
  
Update 5: New version posted (dev19), this one may or may not work with G5s (I don't know yet as I have no G5 machines to test with).
  
Update 6: dev21, this one has a new boot option for scrambling/displaying the root password and starting sshd automatically ("finnix64 sshd")

[0x01] Theoretically, Finnix-PPC should work on OldWorld machines as well, but the setup is a pain. You can try it yourself, but I'm not going to help you to get it working.
