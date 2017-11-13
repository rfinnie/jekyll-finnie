---
date: 2011-01-03 19:20:31-08:00
layout: post
tags:
- planetcanonical
title: 'Identi.ca: lp0 on fire'
wp_id: 1691
---
<div class="video-embed-lg">
<div class="video-embed video-embed-169">
<iframe width="640" height="360" src="https://www.youtube.com/embed/niAme_z9vxI" frameborder="0" allowfullscreen></iframe>
</div>
</div>

Behold, the Panasonic KX-P2023. Introduced in the early 90s and [still in production today](http://www.officedepot.com/a/products/543371/KX-P2023-24-PIN-240-CPS/), the KX-P2023 is a 24-pin parallel dot-matrix printer, capable of handling sheet-fed copier paper or perforated computer paper.

This particular KX-P2023 is 18 years old, bought by my family as part of a computer bundle in 1993, along with a Packard Bell 486SX computer and monitor. When I turned 18 and moved into my own apartment, I took the printer with me, as inkjets were still somewhat expensive, but I eventually bought a replacement printer. Still, over the next decade or so, the dot matrix printer traveled with me as I moved to the west coast, but wasn't powered on for more than a decade. Last year, I dug it out of storage, and tried to think of a purpose for it.

[Identi.ca](http://identi.ca/lpd).

In homage to [Eric's ImageWriter II](http://imagewriter.yikes.com/), I would print out Identi.ca.

The Panasonic's ink ribbon was amazingly still good (remember, it had been in storage for more than a decade), and replacement ribbon cartridges are still easy to find and cheap. And office supply stores still sell perforated computer paper! (The feeder needed for the KX-P2023 to accept regular sheet-fed paper is missing, but not needed for computer paper.)

I set it up in my home office on a printer stand with the feed paper below, attached it to my Linux router (which is the only 24/7 computer in my home to still have a parallel port), and wrote a script to interface with Identi.ca. ([I've had some experience interfacing with Identi.ca.](https://www.finnie.org/2009/07/18/x11r5-joins-twitter-starts-harassing-oprah/))

The printer checks its Identi.ca account, [@lpd](http://identi.ca/lpd), every few minutes, and prints any new dents by its followers. So if you want your dents to be printed out, simply add [@lpd](http://identi.ca/lpd) as a friend. Dents are formatted somewhat, with Panasonic (or maybe Epson?)-specific control codes to overstrike/bold the poster's username, but no attempt at word wrapping is made.

If you want your printout to stand out, @-reply to [@lpd](http://identi.ca/lpd). The script will notice this, overstrike the entire dent, and separate it with double lines and whitespace.

The printer will run from 10AM to 10PM Pacific, seven days per week. (My home office is pretty far away from my bedroom, but still.) Currently any dents sent outside of that window will be lost, but I am working on a queuing mechanism where the printer should try to catch up with the night's dents during the day. [@lpd](http://identi.ca/lpd) will post status updates, letting you know when it is on-line, off-line, on fire, etc.

(The KX-P2023 is not a Postscript-capable printer, so don't bother trying to send it PS commands. I'm currently not filtering escape codes, so if you can find them yourself, you could try, but I think Identi.ca strips out non-text/Unicode data. And on that note, I'm actually not sure what will happen if you send it legitimate Unicode characters.)
