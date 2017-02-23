---
date: 2012-08-14 23:15:59-07:00
layout: post
tags:
- planetcanonical
- twuewand
title: twuewand 2.0 released
wp_id: 2122
---
You may remember [about a year ago](https://www.finnie.org/2011/09/25/introducing-twuewand/) when I released [twuewand](https://www.finnie.org/software/twuewand/), a TrueRand implementation. TrueRand is a hardware entropy generation technique, implemented in software. In a nutshell, it works by setting an alarm for a few milliseconds in the future, and flipping a bit until the alarm is reached. It works due to the fact that time (your computer's RTC) and work (your computer's CPU) are not linked, so the result when the alarm comes due is unpredictable.

TrueRand was invented in 1995, and had mostly been forgotten for the last decade, until I started doing research on it last year. So it was quite a surprise when I was at Dan Kaminsky's talk at DEFCON a few weeks ago, and one of the topics he brought up was TrueRand. ([Go check out his presentation slides](http://dankaminsky.com/2012/08/06/bo2012/); I just want to point out that while I'll be focusing on entropy and debiasing here, he goes into a lot of other interesting topics.)

Dan came to roughly the same conclusion as I did, that entropy sources have gotten worse over time, not better, and systems like VMs are almost completely devoid of entropy. Even more worrying, [a paper published this year](http://eprint.iacr.org/2012/064.pdf) came to the conclusion that approximately 1 out of every 200 public keys on the Internet are easily breakable, not due to weaknesses in the encryption, but by bad entropy being used when generating the keypair. TrueRand may have been forgotten, but it's needed today more than ever. Dan and I talked for awhile after his talk, and went over a few things by email in the week following. twuewand 2.0's new features are influenced by those discussions.

Dan proposed a number of enhancements for TrueRand, mostly centered around other ideas for measuring variances given only a CPU and RTC, but what caught my eye was his idea of enhancing debiasing.

Many forms of random data are random in the technical sense, but are prone to bias. As an theoretical example, take a gun placed in a sturdy mount and pointed at a target not too far away. Most of the time, shots from it will hit the same spot every time (0), but occasionally they won't (1). So you're left something like 00000001000001001100000010000010; mostly hits, but with random misses. So it's random in a technical sense, but the distribution is heavily weighted toward one side.

The simplest method of debiasing is known as Von Neumann debiasing. Bits are processed as pairs, and any pair that is both 0 or both 1 is simply thrown out. Out of the pairs that are left, {0,1} becomes 0 and {1,0} becomes 1. So in the example above, the Von Neumann debiased output would be 00101. The data is now distributed better, but as you can tell, a lot was lost in the process. This is an extreme example since the data was heavily biased to begin with, but in data without a lot of bias, you still lose at least 50% of the bits (I've found 70-75% in real-world twuewand usage).

Dan thought, "Hmm, that's an awful lot of data simply being thrown out. We can't use the discarded data in the direct output, but perhaps we can use it to better (de-)influence the final output." He came up with a method he called modified Von Neumann, which I refer to in twuewand as Kaminsky debiasing.

The incoming bit stream is still run through Von Neumann, and put into an output buffer. However, all bits (whether they pass Von Neumann or not) are fed to a SHA256 stream. Occasionally (after the input stream is finished, or a sufficient number of bytes are put into the output buffer), the SHA256 hash is computed<sup>[1]</sup>, and used as a 256-bit key for AES-256-CBC encrypting the output buffer. This way, only the bits which pass Von Neumann influence the output directly, but all bits help indirectly influence the output as well.

So [twuewand](https://www.finnie.org/software/twuewand/) now supports Kaminsky debiasing, and will use it by default if Digest::SHA and Crypt::Rijndael are installed.

* * *

Now, I want to clear up a mistake I made in my last post. I said that feeding twuewand output to /dev/urandom on Linux systems influences the primary pool, increasing entropy. First, you can actually write to either /dev/random or /dev/urandom, the effect is the same. But more importantly, entropy is NOT increased by writing to /dev/[u]random. It's merely "stirring the pot". If your system is out of entropy and you are blocking on /dev/random, no amount of writing to /dev/[u]random will unblock. (Directly, that is. If you're banging on your local keyboard to do this, you're slowly increasing entropy, but you could be doing the same thing in a text editor, or to /dev/null.)

Unfortunately, there is no way to increase entropy in the primary pool via standard system command line tools or character devices. However, there is a Linux ioctl, RNDADDENTROPY, which does this. So I wrote a small C wrapper, which takes STDIN and feeds it to the ioctl. This requires root of course. The utility is called, boringly enough, rndaddentropy, and is distributed with the twuewand tarball. It will be built by \`make\` on Linux systems.

I must point out that this utility gives you a very excellent method to shoot yourself in the foot. The lack of command line tools to directly access the primary buffer is most likely by design, since this bypasses most of the in-kernel filters for random input. Injecting, say, the contents of /bin/ls to the primary pool would be a great way to become one of those 1 in 200 statistics. Only use this utility to feed high quality entropy (such as by twuewand, or something like [an entropy key](http://www.entropykey.co.uk/)).

* * *

Dan Kaminsky will be publishing software in the future called DakaRand, which does much of what twuewand currently does, but incorporates some of his other ideas. He provided me a work-in-progress copy, which looks very interesting, but it is not available to the public yet for a number of reasons. Be on the lookout for that when it is released.

**Update (2012-08-15):** Dan [released DakaRand 1.0](http://dankaminsky.com/2012/08/15/dakarand/) a few hours after I made this post. Go check it out.

* * *

<sup>[1]</sup> In Dan's proposal, after the SHA256 hash is computed, it would then be run through Scrypt's hashing algorithm. This is not done in twuewand for two reasons. First, Crypt::Scrypt does not currently provide a low-level method to just do hashing; instead it wants to create a full digest which is unsuitable for this purpose. Second, Dan has been debating whether this step is necessary or desirable at all, and Scrypt has "undefined effects on embedded hardware".
