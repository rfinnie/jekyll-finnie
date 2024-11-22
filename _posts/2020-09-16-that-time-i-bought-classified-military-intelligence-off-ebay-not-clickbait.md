---
date: 2020-09-16 10:45:57-07:00
excerpt: 'TODO: Add more arrows'
excerpt_standalone: true
headline_image: /blog-media/2020/cobalt-raq2-classified.jpg
layout: post
title: That Time I Bought Classified Military Intelligence Off eBay (NOT CLICKBAIT)
---
Yesterday, "Alex" posted a [hilarious account](https://mango.pdf.zone/finding-former-australian-prime-minister-tony-abbotts-passport-number-on-instagram) of getting former Australian prime minister Tony Abbott's passport information. The lesson of the story boils down to: how do you report a vulnerability at a national scale? Do you just call 1-800-NATIONAL-SECURITY and let them know what happened? (Actually, Australia does have a literal hotline like that, but it turned out to be useless. Seriously, before you read further here, go read "Alex"'s story.)

This story reminds me of a similar problem I had 13 years ago. But be warned, this story is completely from memory and some things may be wrong. It will also not be nearly as entertaining as "Alex"'s story.

<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2020/cobalt-raq2-classified.jpg" alt="Cobalt RaQ 2, redacted" class="img-responsive img-rounded img-lg">

In 2007, I bought a [Cobalt RaQ 2](https://en.wikipedia.org/wiki/Cobalt_RaQ) off eBay. Introduced in 1999, the RaQ 2 was the rackmount server variant of the more well known [Cobalt Qube 2](https://en.wikipedia.org/wiki/Cobalt_Qube). It featured a MIPS R5000 CPU and was technically a general purpose server platform, but was most often used as a file server or proxy server. I bought it used (and long after its useful life) because I like to collect non-x86 hardware which can run Linux.

When it arrived, I powered it on, intending to look around Cobalt's bespoke Linux distribution before wiping the drive and installing Debian. I was expecting a server restored to factory settings, but was secretly hoping for some random company's fileserver data. Likely an out-of-business company since these sorts of sales usually come from liquidators.

Through the front panel, I was able to reset the administrator password and retrieve the configured IP information, which was... a public IP address. Huh, weird.  Was it being used as a web server?  Doing a whois on it revealed it was in a block owned by the **DoD Network Information Center**. Yes, that DoD.

By attaching a crossover Ethernet cable, I was able to log into the RaQ's administrative web interface.  I could see it was configured as a caching LAN web proxy, and its hostname ended in **smil.mil**. Yes, that military.

Without looking at the contents, I could tell that the drive was full of cached content, with the usual extensions (.html, .jpg, etc), though if I remember correctly, the base filenames were randomly assigned so there was no indication of the subject of each file's content. The file dates were mostly through 2003.

That's as far as I dug.  I turned off the RaQ and pulled the hard drive.  As I would come to learn, smil.mil is a domain on [SIPRNet](https://en.wikipedia.org/wiki/SIPRNet), the US government's classified equivalent to the Internet. SIPRNet is supposed to be air-gapped; there should be no physical way to reach a machine on SIPRNet from the Internet or vice versa, and it was definitely not accessible to civilians.  The fact that I had a caching proxy server used on SIPRNet was inconceivable.

(This also explains why it had a "public" IP address. If you look at [visual representations of IPv4 space](https://vad.solutions/ipmap/) (buy this poster, hint hint), you'll see DoD spaces but they don't appear to be in use. They *are* in use, just not on the Internet. RFC 1918? Never heard of it. Similarly, smil.mil and sgov.gov look like Internet domains, but only resolve on SIPRnet. I'll give the government a pass here; the IANA has no organization-specific private namespace, like .internal or .lan, and it annoys me that domains like that are not explicitly reserved. What were we talking about? Oh right, national security.)

So what should I do with this information?  I could go to the press, and it would be quite a embarrassment to the administration, but I didn't want to draw political attention to myself.  Ideally I just wanted to go to someone official and say "someone messed up, here's the server's hard drive and who I bought it from". But again, there isn't really a 1-800-CLASSIFIED-LEAK.

A co-worker suggested I contact my congressman's office to see if they could help me navigate to someone proper in the federal government. I called, they took down my information, and never called me back.  I sent an email, no response. I'm sure I sounded like a crackpot, but it's not my fault it was all true.

In the end, I gave up. The hard drive went into my document safe, just in case I got a call later, perhaps the FBI got a list of buyers from the eBay seller.  But that never happened, and about 5 years later I destroyed the hard drive without loading any of its cached contents.  Somehow I lost my email archives from earlier than 2010, and eBay sale records only go back about 2 years, so I can't even find out who sold me the server back in 2007.

I still have the Cobalt RaQ 2 hardware today (minus the hard drive), and it currently runs Debian off an SD to IDE adapter. There was nothing otherwise special about the hardware, nor any asset stickers or similar, so now it's just a weird server from an old generation.
