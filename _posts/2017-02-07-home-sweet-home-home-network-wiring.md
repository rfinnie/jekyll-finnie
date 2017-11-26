---
date: 2017-02-07 13:41:40-08:00
excerpt: Wi-Fi may be everywhere. But back in the day, when men were men, women were
  women and networks were classful, we did things with wires.
headline_image: /blog-media/2017/wall-keystone.jpg
layout: post
tags:
- homesweethome
title: 'Home sweet home: home network wiring'
---
<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2017/wall-keystone.jpg" alt="Keystone 4 port wall plate" class="img-responsive img-md img-rounded pull-right">These days, the average person's "home network" is a Wi-Fi router (provided directly by the cable company, as I see from most of the APs within range), with a laptop, a smartphone, and maybe a few "things".
But back in the day, when men were men, women were women and networks were classful, we did things with wires.

In my [previous part in the series]({{ site.url }}{{ site.baseurl }}{% post_url 2017-02-05-home-sweet-home-light-bulbs %}), I mentioned that one of my longtime goals was to have Decora light switches, something easily obtainable once I had my own house (literally done on the first day).  Another even older goal was to have a whole-house Ethernet network.  Back in the 90s, I was jealous when two of my friends rented a house and were able to string Cat5 everywhere.  Throughout the various apartments I lived in, the network was usually a wired network in the home office, and sometimes an AP in client bridge mode to connect the living room to the office.  In my last apartment, I was lucky enough that the wall behind the TV in the living room abutted the office, so I drilled a small hole and ran a cable through it, and patched it up again when I moved out.

But when you throw over a quarter of a million dollars toward home ownership, you get an actual house!  You can [cut holes into walls](https://xkcd.com/905/) without upsetting the landlord!  Exciting times!

My first *significant* home improvement project was about a week after signing: wiring the house for Cat5e.  Each room already had RJ-11 (telephone) and coax running to it, but in almost all of the rooms, the RJ-11 was a baseboard "biscuit block", and the coax was just drilled through the edge of the floor.  The RJ-11 all ran through the crawlspace and terminated in a jumble of wires at the NID (telco demarcation point) on the side of the house.

I ripped all of that out and spent two days crawling through the crawlspace, running Cat5e and repositioning the coax.  Each room had a new hole cut into it, with a [low voltage mounting bracket](https://www.monoprice.com/product?p_id=7013) and a [4-port keystone wall plate](https://www.monoprice.com/product?p_id=6731).  One of the keystone jacks would be for the coax, and the remaining three for Cat5e.

<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2017/distribution-rack.jpg" alt="Keystone 4U distribution rack" class="img-responsive img-md img-rounded pull-right">As luck would have it, the laundry room had the perfect place for mounting a distribution panel.  I mounted a [4U wall mount bracket](https://www.monoprice.com/Product?p_id=8627), a [shelf](https://www.monoprice.com/product?p_id=8631), a [32 port keystone patch panel](https://www.amazon.com/gp/product/B0002J1NDC) and a [16 port 1U managed switch](https://www.amazon.com/ZyXEL-16-Port-Gigabit-Ethernet-Managed/dp/B00H1OM0BA), and ran all of the Cat5e to it.

When working in an under-house crawlspace, having two spools of cable really helps.  Three would have been even better since each room was getting three feeds, but I couldn't personally justify that.  Two-way radios also help for communication between the crawlspace and the house to coordinate where to drill and when to feed cable.  (It didn't help that the person helping me was legally deaf.  True story.)

In addition to the runs to the rooms, I ran two feeds from the patch panel directly back to the NID, but those are only attached on the patch panel side.  I have no current need for telco services (internet is cable, and home telephone is optional in my case, but I do have VoIP for the novelty of it), but if the need arises, it can be easy to manage.  All I would need to do is attach one of the pairs in one of the feeds at the NID, and patch it in at the distribution panel.  This configuration could also allow for easy FTTP (fiber to the premises) integration, if that ever becomes an option in my area.

The coax still goes directly to the cable company's demarcation box on the side of the house, as that's fine management-wise.  The cable company's box is large enough to comfortably terminate 5 coax feeds, and you don't need to reconfigure the layout as often as with an Ethernet network.

Sometimes people ask why I went with Cat5e in 2014, as opposed to Cat6a or Cat7 for 10Gbps Ethernet.  Basically, it's expensive, more expensive than I could justify.  While there's something to be said for future-proofing, the cable and jacks were (and still are) many times more expensive than their Cat5e equivalents.  But if the need ever arises in the future, the holes are drilled and cut, the physical mounts are all mounted and everything is keystone, so it wouldn't be too difficult to replace the wiring.

---

The actual logical network is far less interesting IMHO, but worth mentioning.  Internet come in via cable to a cable modem, then to what I call the Omniserver: an Ubuntu PC with 3 GigE ports, i5-4690K, 32GiB RAM, boot SSD and 4x 2TB drives in RAID10.  This system is the current evolution of an effort to centralize what used to be a half-dozen machines.  It's the router/firewall, file server, and has a handful of VMs for development, testing and a single Windows VM (which literally just runs Quicken).

One Ethernet line comes in from the cable modem, two go out to the office switch.  Both the office and distribution rack switches are managed (ZyXEL GS1900-24E and GS1900-16, respectively), and I have plenty of ports, so I may as well take advantage of them with bonding, even if I don't strictly need it.  The link between the two switches themselves are also dual bonded.

Wireless comes in the form of an ASUS RT-AC87U 802.11ac access point.  Interestingly, the case has a label on the first and second switchports which says "teaming port", but the internal switch chipset doesn't physically support bonding, and it's not mentioned in any of the advertisement specs or documentation.  I'm guessing it was an intended feature during development but was cut close to release.
