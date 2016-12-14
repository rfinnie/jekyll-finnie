---
id: 1725
title: More IPv6 musings
date: 2011-02-04T02:20:24+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/2011/02/04/more-ipv6-musings/
permalink: /2011/02/04/more-ipv6-musings/
openid_comments:
  - 'a:1:{i:0;i:330095;}'
categories:
  - Uncategorized
tags:
  - planet:canonical
---
A local ISP, Great Basin Internet Services, was allocated a /32, [2607:f1a8::/32](http://whois.arin.net/rest/net/NET6-2607-F1A8-1), an inconceivable number of IPs. My first thought was, "gee, the IANA is already over-allocating and not learning from history". However, doing the math starts to put things back in perspective:

  * IPv6 is technically CIDR, but is logically aligned to certain sizes.
  * A normal internet user like you or I would get a /64, which is the standard subnet mask in IPv6. Since IPv6 is a 128-bit space, that's 128 - 64 = 64 bits, 2^64 usable IPs (broadcast and subnet addresses don't exist in IPv6), approximately 18 sextillion addresses, or 18 million billions.[0] That seems like an immense number of IPs for personal use, but there's a good reason for that. It's designed so that EUI-48 (MAC addresses) or EUI-64 (Firewire addresses, etc) can fit inside the range automatically, which allows for various autoconfiguration schemes.
  * An organization is assigned a /48, which allows the organization to re-allocate /64 portions of it out to things like multiple offices, etc. 2^(64-48) = 65,536 possible re-allocations from organization to standard subnet.
  * An ISP is assigned a /32, which allows them to re-allocate 2^(48-32) = 65,536 /48s to organizations, or 2^(64-32) = about 4.3 billion /64s to individuals, or a mix of the two.
  * So now we're down to the /32 level. Like IPv4, IPv6 contains some pockets of globally unusable networks: fc00::/7 (unique local), fe80::/10 (link local), and ff00::/8 (multicast). Everything else is theoretically usable, so we lose 2^(32-7) + 2^(32-10) + 2^(32-8) = approximately 54.5 million ISP-level /32 allocations out of a possible 2^32 = approximately 4.3 billion allocations.[1]
  * However, the IANA has reserved for future use all but one block: 2000::/3. That makes 2^(32-3) = approximately 537 million immediately allocatable ISP-level /32 blocks. That's still a lot.

So that breaks the whole "number of stars in the sky"-type analogies down into meaningful chunks. A carrier-grade ISP with a single allocation could support 65,536 clients, a consumer ISP could support about 4 billion users, and about 500 million ISP allocations could be made today, with billions more possible in the future. I could see 2000::/3 possibly being used up during my lifetime, but there's another 8 or so of those-sized chunks ready for allocation if needed.

[0] These are short scale -- eat it, Queen of England!
  
[1] There are some other areas of IPv6 that are technically globally routable but not allocatable, but I'll gloss over those in the name of simplicity.
