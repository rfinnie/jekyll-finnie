---
author: Ryan Finnie
categories:
- Uncategorized
date: 2009-11-27 23:36:06
enclosure:
- '/blog-media/uploads/2009/11/outside-line.wav

  51898

  audio/x-wav

  '
guid: http://www.finnie.org/2009/11/27/outside-dial-tones-on-spa500-series-phones/
id: 1214
layout: post
permalink: /2009/11/27/outside-dial-tones-on-spa500-series-phones/
title: Outside dial tones on SPA500 series phones
---
[<img src="/blog-media/2009/11/photo-225x300.jpg" alt="Cisco SPA504G" title="Cisco SPA504G" width="225" height="300" style="float: right; margin: 1em;" />](/blog-media/2009/11/photo.jpg)I'm currently in the process of upgrading our phone systems at work. In the Reno and Salt Lake City offices, we had a [Cisco Unified Communications](http://www.cisco.com/en/US/netsol/ns151/networking_solutions_unified_communications_home.html) VoIP system going back to 2001, with [Cisco 7900 series](http://www.cisco.com/en/US/products/hw/phones/ps379/index.html) phones. The 7900s used SCCP, a proprietary but decently understood protocol to talk with the call managers. Earlier this year, the call managers died. We had been preparing for this possibility, and had an [Asterisk](http://www.asterisk.org/) system ready that was able to talk SCCP to the 7900 phones. Unfortunately, the SCCP driver is graciously described as "beta" (remember, it's a proprietary protocol). Everything mostly worked, but we lost the ability to do 3-way conferencing via the phones. So now we're replacing them with [Cisco SPA504G phones](http://blog.voipsupply.com/first-look-cisco-spa504g-ip-phone), which use the industry standard SIP protocol.

In the last few weeks/months, I've been preparing to make this process as smooth as possible from a user perspective, since this is the most visible aspect to the employees. One of the features of the 7900 phones is a separate tone when reaching an outside line. When you pick up the phone, you hear a standard North American dial tone (350 + 440 Hz). When you press 9, however, that changes to a different dial tone, to signal that you are "outside" and can dial as if you were at a regular POTS phone. [Here is a sample audio file of that process.](/blog-media/2009/11/outside-line.wav) I always found the order to be backwards; I figured the "inside" dial tone should be non-standard, and once you hit 9, you'd be presented with a standard North American dial tone. Oh well.

The SPA504G can be programmed to generate an outside dial tone, but it defaulted to a single 440 Hz tone, which just did not sound right. It's customizable, so I started looking for what DTMF combination the 7900's outside dial tone is. I didn't find anything online, and while many of the tones on the 7900s are sent to the phone by the call manager, the outside dial tone is hard-coded in the phone's firmware. I could have probably figured it out if I had an oscilloscope, but I didn't have access to one. Random fiddling came close, but it still sounded off.

Through some reading, I eventually figured out that "special tone frequencies" are always in multiples of 40, 50 and 90 Hz. This apparently makes it easier to determine what two tones are being sent in a DTMF sequence. A dial tone is 350 + 440 Hz (90 Hz difference), a ringing tone is 440 + 480 Hz (40 Hz difference), and a busy signal is 480 + 620 Hz (90 + 50 Hz difference). With that, I was able to narrow down the possibilities, and came up with 440 + 530 Hz for Cisco's outside dial tone. The low tone is 90 Hz higher than the 350 Hz low tone of the dial tone, and the high tone is 90 Hz higher than the low tone.

If you happen to be in my same situation (or if you just want to use an outside dial tone that sounds better than that stupid single 440 Hz outside dial tone that the SPA504G defaults to), go to Admin, Advanced, Regional, Call Progress Tones, Outside Dial Tone, and use this:

<pre>440@-19,530@-19;10(*/0/1+2)</pre>

Note that to actually get the phone to play an outside dial tone, you must configure your dial plan to support it. That means that after every 9, enter a comma. Here's my current dial plan:

<pre>(3xxx|*22xx|*23xx[0-9*#].|*2x|*3xxx|9,11|9,911|[346]11|0|9,[2-9]xxxxxx|9,1[2-9]xx[2-9]xxxxxx|9,011xx[0-9*#].)</pre>

Note that **all** leading 9s need to have a comma after for this to work. See, for example, `9,11|9,911`. The user can still either dial 911 or 9911 in an emergency, it's just the outside dial tone will play after the first 9.

By the way, for that example wav file I showed above, I had considered recording the actual sound coming from the phones with a microphone, but I decided to do it all in [Audacity](http://audacity.sourceforge.net/) instead. That's right, it was completely computer-generated. All I had to do was use Audacity's tone generator on separate channels (for example, one channel with a 350 Hz tone and one channel with a 440Hz tone for the regular dial tone), and then combine everything down into a single mono channel.
