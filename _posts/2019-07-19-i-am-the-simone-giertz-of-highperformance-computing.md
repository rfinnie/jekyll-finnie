---
date: 2019-07-19 11:17:30-07:00
excerpt: Wherein Ryan builds a creative waste of electricity
excerpt_standalone: true
headline_image: /blog-media/2019/cluster-hat-rng.jpg
layout: post
title: I am the Simone Giertz of high-performance computing
---
It all began with a tweet.

<blockquote class="twitter-tweet" data-cards="hidden" data-lang="en"><p lang="en" dir="ltr">Maybe I should finally make a cluster so that high school me is no longer disappointed in me. <a href="https://t.co/D5tiKORh2D">https://t.co/D5tiKORh2D</a></p>&mdash; Brian Danger Hicks 🏴‍☠️ (@crazyunlikeafox) <a href="https://twitter.com/crazyunlikeafox/status/1147209037630574592?ref_src=twsrc%5Etfw">July 5, 2019</a></blockquote>

I too spent my late teens and early 20s thinking clusters were the future.  I gawked at a friend who worked on an SGI Altix in college.  I wanted a Beowulf, whatever those were.  Itanium!  Blades!  Infiniband!

And yet somehow I failed to notice over time that clusters WERE the future, and that they have become my career.  I build and maintain million-dollar clusters with thousands of instances running parallel scale-out workloads.  I think what caught me off guard is that they're called "clouds" now.

Anyway, the Raspberry Pi Cluster Hat allows you to cluster up to four Raspberry Pi Zeros to a regular Raspberry Pi.  I can't think of a single instance where this would be useful.  Zeros are amazingly slow; a cluster of four combined have half the CPU performance of a single Pi 2, not even to say the 3, 3+ or 4.  The original Zero didn't have any sort of networking so this would allow for communication (via USB gadget mode), but the Zero W has built-in Wifi.  Even if you already had a regular Raspberry Pi and four original Zeros and wanted to combine them together, the hat costs $50, whereas a brand new Pi 4 would be much faster and only $35 (plus dongles).

The Cluster Hat is a product which should not exist.

I love that it does exist.

I ordered one.

I also ordered four Zero Ws.  Problem is while they're theoretically $10 each, they're very hard to find individually and always have strict one per person limits.  So I bought [four kits at $25 each](https://www.amazon.com/gp/product/B0748MPQT4/) even though I didn't have any need for the included accessories.  And four $7 32GB MicroSD cards.  And [the hat itself](https://clusterhat.com/).  Including the original purchase of the Pi 2 several years ago, this all adds up to $170.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Let’s be honest, the main reason to build a cluster like this is the blinkenlights. <a href="https://t.co/uyAYo5HjUG">pic.twitter.com/uyAYo5HjUG</a></p>&mdash; Ryan Finnie (@fo0) <a href="https://twitter.com/fo0/status/1152050345167618048?ref_src=twsrc%5Etfw">July 19, 2019</a></blockquote>

With the parts on the way (the hat was shipped from the UK and took about two weeks to arrive), I pondered what to do with the cluster.  It had to *do* something; creative irrelevance needs to have a grain of relevance to be amusing.  I did know I wanted to have some sort out on-device output, so I also bought [a $25 2.13" e-paper hat](https://www.amazon.com/gp/product/B071S8HT76/) to attach to the outermost Zero.  Running total: $195.

By the time the parts arrived, I had written the code for the cluster.  I now have an overcomplicated random number generator.

<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2019/cluster-hat-rng.jpg" alt="Raspberry Pi Cluster Hat random number generator" class="img-responsive img-rounded img-lg">

TrueRand is a topic [I've written about a few times](https://www.finnie.org/2012/08/14/twuewand-2-0-released/) here.  It's a software-based hardware random number generation technique which relies on the unpredictable interaction between a computer's CPU and RTC.  Basically, a bit is flipped for a certain amount of time, recorded, and debiased.  I used my own software, [twuewand](https://www.finnie.org/software/twuewand/), which tries to target a certain number of flips (40,000 by default) but will rarely get exactly 40,000, so this actually makes generation scale with CPU power.

After debiasing, the four nodes combined produce about 5 bits per second.  Yes, bits.  Per second.  This works out to 4 bytes per 6.4 seconds, which are displayed on the e-paper display.  These four nodes' outputs are collected on the main Pi, which sends commands back to the 4th node for what to render on the e-paper display.

I now have a miniature space heater which conveniently gives me truly(-ish) random bytes at a glance.

I could have used the Raspberry Pi 2 to run twuewand (which automatically uses all cores available on a single node) to get about 12 bits per second.  Or 240 bits per second on my laptop.  Or I could have used the SOC true-hardware RNG on the Pi 2 which seems to do about 1 megabit per second.

But then where's the fun in that?
