---
date: 2010-12-11 21:12:42-08:00
layout: post
tags:
- planetcanonical
title: 'vanityhash: Brute forcing hashes for fun and profit'
wp_id: 1656
---
<pre><strong>0cbbc</strong>83f4f9dc19f55b9e15287a8a7f4  bbc-2.1.iso
<strong>f100ff</strong>a1d470b7d119979c58f18bb6b1  finnix-100.iso</pre>

In 2003, [LNX-BBC](http://www.lnx-bbc.com/) 2.1 was released. The final ISO just happened to have an MD5 hash that began with "0cbbc", which several developers noticed; pretty good karma. But Seth Schoen realized it would be possible to brute force a desired hex fragment relatively quickly, by appending a few random bytes to the end of the ISO, and searching until a desired fragment was found. (Extra garbage data at the end of an ISO is harmless, and ignored when mounted.) Schoen later released hash_search, a utility that went through a search space (up to 32 bits) and looked for hashes that matched a given pattern at the beginning. Futher development builds all started with "bbcbbc". (Sadly, LNX-BBC never made a final release past that, though the plan was for LNX-BBC, say, 2.2 to be released with "bbc220" at the beginning of the MD5 hash.)

The way it works is pretty simple. MD5 (as well as other hashes) process data in a stream. You can add the original input data to an MD5 context, remember the context's current state, clone the context, add test data, and perform a hash on the current cloned context. If the cloned context doesn't contain the fragment you want, simply go back to the original context, clone it, and try again. Effectively, you only have to read the original data once, and you are using processing power to only hash a few bytes, rather than the whole input data. On today's hardware, an MD5 checksum of a 100MB ISO may take a few seconds, but running hash_search on a a 100MB ISO can run through hundreds of thousands of hashes per second.

I re-discovered hash_search a few months ago, and used it when releasing [Finnix 100](http://www.finnix.org/), whose MD5 hash was <tt>f100ffa1d470b7d119979c58f18bb6b1</tt> (notice <tt>f100ff</tt>).

However, hash_search had a few drawbacks. It wouldn't compile on a modern GCC (though was easily fixable), and was missing a number of features that would be nice today. I had a few email conversations with Schoen last month where we discussed some ideas, and I decided to build a replacement.

The result is [**vanityhash**](http://www.finnie.org/software/vanityhash/).

vanityhash is a superset of hash_search. It has two modes. By default, it will take input data, and go through a given search space (24 bits by default), and list all possible matches. It can also take input data, go through a search space, and for the first match it finds it outputs the original input, plus the extra data needed to produce the desired hash. If no match is found in a search space, it simply outputs the original input.

In addition, vanityhash supports the following new features:

  * Parallelized execution, supporting multiple processors/cores/threads.
  * Search spaces up to 64 bits. Search spaces larger than 32 bits require a 64-bit OS.
  * Support for MD2/4/5, SHA1/224/256/384/512.
  * Ability to match in parts of the hash other than the beginning.
  * Ability to match any part of the hash.

vanityhash is slightly slower than hash_search, but the added features make up for it. With my fastest machine, I can search a 24-bit search space (which is likely to contain at least one 6 hex digit match) in 12 seconds. A 32-bit search space (which is likely to contain at least one 8 hex digit match, or multiple 6 hex digit matches) can be done in less than an hour.

The idea is to continue adding vanity hashes to Finnix releases going forward (<tt>f101ff</tt>, <tt>f102ff</tt>, etc).

There is a security concern that is pointed out by a tool like this. Even I have a tendency, when verifying hashes, to only compare the first few digits of a hash. This tools proves that it is now technologically trivial to produce an arbitrary partial hash of at least 6 digits. Better hashing algorithms aren't the answer (but are in general a good idea), since matching against SHA512 is just as easy as MD5. Be sure to verify the entire hash, or even better, use a cryptographically-signed checksum such as GPG. (All Finnix releases are GPG-signed.)
