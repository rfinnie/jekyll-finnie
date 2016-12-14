---
author: Ryan Finnie
date: 2010-12-27 00:04:55
guid: http://www.finnie.org/2010/12/27/postal-system-abuse-for-fun-and-profit/
id: 1671
layout: post
permalink: /2010/12/27/postal-system-abuse-for-fun-and-profit/
title: Postal system abuse for fun and profit!
---
I hang out on a smallish IRC channel which is frequented by people from several countries: the U.K., the Netherlands, Denmark, Australia, New Zealand, Brazil, and probably more countries I do not remember. There are several of us from the US, but we are in the minority. "In my country, we do things _this_ way" conversations happen frequently.

A few weeks ago, a "world geography" quiz I found and pasted eventually led to a discussion of postal addresses. A friend mentioned:

> <bz2> If you want to mail something to me, "NL-2333 AP ██" should be enough.

NL is the Netherlands, obviously. 2333 is a postal code, somewhat like a ZIP code in the US. 23 is the town (Leiden), 33 is a neighborhood in Leiden, AP is an identifier for what is usually a street within the neighborhood, and ██ is the house/unit number (protected for his privacy).

The long form would look like this:

> IRC user bz2
  
> Flanorpad ██
  
> 2333 AP Leiden
  
> The Netherlands 

And indeed, this is the exact format I would normally use to send him international mail. But the short form is recognized and is often used as the return address, minus the country for domestic mail (bz2 said he simply uses "2333 AP ██").

A shorthand for US postal addresses simply doesn't exist, however. My full international address is:

> Ryan Finnie
  
> 4959 Talbot Lane #███
  
> Reno, NV 89509-6521
  
> United States 

This official format mostly evolved from historical use, but logically it is quite random. The logical ordering doesn't flow in one direction to another (smallest to largest or vice versa). But I'll explain in order of largest to smallest. United States is self-explanatory. NV is the state of Nevada. Reno is the city. 89509 is a ZIP/postal code within Reno. (ZIP codes can span multiple cities, but are geographically contiguous. They flow from east to west, so 0XXXX is on the east coast and 9XXXX is on the west coast.) Talbot Lane is a street in Reno (but streets can also span multiple ZIP codes). 4959 is the house number on Talbot. But in this case, 4959 Talbot Lane specifies a sprawling apartment complex of 300 units, so #███ is the unit number.

That leaves one piece of information, 6521, which is the ZIP+4 portion of the ZIP code. ZIP+4 is a optional component of the address, introduced in the 1980s to make machine sorting easier and more accurate. These days it's primarily used by large companies and bulk mailers, in order to get a lower bulk rate. However, ZIP+4 never took off in the general populace; most people don't even know their ZIP+4.

Sadly, ZIP+4 isn't a unique identifier. It's basically designed as a better automated system to get the mail on the right delivery truck, but can still cover a dozen or so addresses. In my case, since 4959 Talbot Lane is such a large apartment complex, it spans multiple ZIP+4s, 19 to be precise, all within 89509. 89509-6521 covers apartments 246 through 264, according to the USPS web site.

With that information, I realized "US 89509-6521 2██" is a globally unique address. It's not "valid" per se, but could be deciphered to my home. The United States Postal Service has a lot of rules for addressing, but is also well known for its resiliency. You could address a letter to "Bob's house at the end of ABC street in Chicago" with no return address, and it's likely that it would (eventually) arrive as long as it had sufficient postage. Apparently the UK Royal Mail is similar; I remember seeing an article (which I can no longer find, unfortunately) about a woman in London who would send letters to herself, with the address encoded in puzzles. Nearly all of them would eventually arrive, with the puzzles hinting to the address filled out. Sometimes it would take days, sometimes months.

So I decided to mail bz2 some [Finnix stickers](http://www.finnix.org/Free_stickers), in the following envelope:

[<img src="/blog-media/2010/12/leiden-envelope-300x168.jpg" alt="" title="NL-2333 AP ##" width="300" height="168" class="aligncenter size-medium wp-image-1672" srcset="/blog-media/2010/12/leiden-envelope-300x168.jpg 300w, /blog-media/2010/12/leiden-envelope-1024x576.jpg 1024w, /blog-media/2010/12/leiden-envelope.jpg 1280w" sizes="(max-width: 300px) 100vw, 300px" />](/blog-media/2010/12/leiden-envelope.jpg)

No names, and both the sender and recipient addresses are not in the format the USPS wants. There is sufficient postage to mail the Netherlands, but the address is internationally non-standard, as far as the USPS is concerned. However, both the sender and recipient addresses are globally unique, and could be figured out by a human.

I'm almost certain this letter will make it, it's just a question of how long it will take. If the USPS's Skynet-level of automation doesn't figure out that "NL-2333 AP ██" is an address in the Netherlands, it will most likely be flagged for review by a postal worker, who should be able to figure it out quickly and actually get it to the Netherlands. From there, the Netherlands' postal service (TNT Post) should have no problem with the short form. bz2 has promised to let me know if/when it arrives, if it's been opened, etc. I put the long form of both my and his addresses INSIDE the envelope, just in case.

What I think would be more interesting would be the reverse. Remember, the short form of bz2's address is legal in the Netherlands. But the address I listed as a return address is not strictly legal in the US. I'll see if I can get him to mail a letter to me using that form, which would be a more interesting test of the USPS.
