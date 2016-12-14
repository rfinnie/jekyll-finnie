---
id: 606
title: All about the FrankenDell
date: 2006-08-07T19:58:00+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/2006/08/07/all-about-the-frankendell/
permalink: /2006/08/07/all-about-the-frankendell/
lj_itemid:
  - "157641"
lj_import_url:
  - http://fo0bar.livejournal.com/157641.html
categories:
  - Uncategorized
---
<div class="flickr-frame">
  <a href="http://www.flickr.com/photos/fo0bar/208260148/" title="photo sharing"><img src="http://static.flickr.com/92/208260148_db13ea47f8.jpg" class="flickr-photo" alt="" /></a><br /> <span class="flickr-caption"><a href="http://www.flickr.com/photos/fo0bar/208260148/">FrankenDell</a>, originally uploaded by <a href="http://www.flickr.com/people/fo0bar/">fo0bar</a>.</span>
</div>

I told you I'd give you a story, and now you will have a story.

At previous Defcons, someone would usually bring a WRT54G or equivalent for the hotel room. We would plug it into the hotel RJ45 port at the Alexis Park and have mad PAT action, bypassing the per-MAC fees on the internet access and "only" pay $10/day.

This year, I "volunteered" for the task of bringing network equipment. I was driving this year, so I went a little overboard: access point, switch, FrankenDell server/router, many network cables, and various small gear. And a cooler for Coronas. I even brought a $20 CompUSA AP/router just in case everything went to hell. I thought I did a good job on the server, with the usual services (DHCP, DNS, etc), and even an OpenVPN connection that would route/encrypt everything to my server in San Jose, just in case hax0rz were listening on the hotel network. BTW, the term "FrankenDell" came from the fact that I pieced it together from odd pieces of about 40 workstations that we were about to donate to charity.

Well we got to our room at the Riviera (Alexis Park had MUCH nicer/bigger rooms, but the Riviera's nicer/bigger convention space makes up for it), and I start unpacking gear. I got the server out, plugged in a few cables, and started looking for the RJ45 port for ethernet. And looked. And looked some more. Eventually I find the "wireless internet access for only $9/day!" card on the room table. We collectively thought of 2 things: 1) aww crap, we didn't think that the hotel internet access would be wireless, and 2) that network is going to be pwned pretty quickly, one way or another. And because of #2, we weren't too inclined to put a credit card # into the captive portal page that came up when one of us associated with the network.

Edgan got to work examining the wireless network's infrastructure, while I began looking at ways to modify the FrankenDell to be able to route stuff through the wireless network. We could route stuff through somebody's laptop, but nobody liked the idea of turning their laptop into a full-time router. We could turn our own AP into a bridge client and plug it into the "outside" interface of the FrankenDell, but unfortunately the AP was a WRT54G, and only WAP54Gs could natively bridge. We could flash the WRT54G with OpenWRT and let it do bridging, but that requires internet access and is, like, hard. We could attach a USB wireless port to the FrankenDell, but all we turned up is a WUSB54G that required ndiswrapper for Linux, and the Frankendell has neither internet to download ndiswrapper, nor ndis drivers for loading into ndiswrapper, nor development tools/kernel headers to compiel ndiswrapper. At this point, a few of us decided we need to find a better USB wireless adapter. To Fry's!

In the meantime, Edgan got a few details. The captive portal box is running Mandrake (will probably get rooted), and unauthenticated, only TCP ports 80 and 8080 were open, both redirecting to the captive portal software. Also, UDP port 53 was open to the outside world, unmolested. Woohoo! We could run OpenVPN to a server that is listening on port 53! (OpenVPN rocks because it just requires UDP. No GRE or anything fancy.) However, chicken/egg problem... Can't get outside to set up a server on port 53 without the VPN. However... Edgan checked port 53, but did he check other... no! All UDP ports are available unauthenticated! I had OpenVPN on my laptop, so I fire it up, and sure enough, I now had free internet access. Now, for the others... We jump in the nuclear-powered Prius and drive to Fry's.

At Fry's, we head for networking, and call ghz to google various network cards. We were ideally looking for an old prism2 or hermes-based card, either in USB or half-height PCI. However, all we found were new crap which either had a broadcom chipset (FrankenDell didn't have 2.6.17, and development tools/kernel headers would take a long time to download), atheros chipset (same deal with kernel headers for madwifi), or even worse, we weren't sure since vendors change chipsets frequently and don't list the version on the box art. No 802.11b-only cards to be found. At the last minute, I thought of using a PCCARD PCI adapter, as I had an orinoco silver card in my laptop bag, but I was unlikely to find an adapter at Fry's. But pdx6 stepped in and said he saw one in the Mac aisle. Sure enough, a full PCCARD adapter, not just a dumb wireless bridge for a certain card. It was full-height, but we could just set up the FrankenDell with the case opened slightly.

We brought it back to the hotel room and opened the case... only one problem: the adapter plus orinoco card extended WAY past the end of the case, so while the adapter itself could be fit in the PCI slot, there was no way of getting the orinoco card in. However, the machine was basically expendable, so we could hack off the back of the case. But... all we had in the way of tools was a screwdriver; a Dremel would have been nice. 20 minutes, 1 screwdriver, a lot of swearing, and some blood (ghz's) later, we got enough of the case hacked off to fit the entire card into the case. Booted it up, Debian found the PCCARD bridge and Orinoco card just fine (even gave it eth1, the same interface that the former wired card had, so I didn't have to change any of my firewall scripts), and OpenVPN worked.

So that's it. We bastardized a Dell case, and got free in-room internet access as a result. With our little case mod, we inadvertantly redefined the word FrankenDell for this case. The VPN worked fine, and the hotel wireless was amaingly up for a good 80% of Defcon. Folks, this is the kind of story you tell your grandchildren.
