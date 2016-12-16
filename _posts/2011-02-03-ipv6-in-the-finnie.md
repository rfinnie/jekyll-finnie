---
date: 2011-02-03 23:24:14-08:00
layout: post
tags:
- planetcanonical
title: IPv6 in the Finnie
wp_id: 1718
---
My colocation provider has offered native IPv6 for the past year, and I've been a happy user. The only hiccup occurred a few weeks ago when they went down for about a minute or two completely, but for the next hour after IPv4 service came back up, IPv6 was still down. Other than that, no problems. They're based in San Jose, and appear to be peered with Hurricane Electric, the people who run [tunnelbroker.net](https://www.tunnelbroker.net/), so network-wise, they're pretty close to many US IPv6 users.

I'm about 99% done migrating to full IPv6 availability on my colo box. Web serving for my major sites (finnie.org, finnix.org, etc) have been IPv6 enabled for almost a year now, and while all web my web sites are technically IPv6 available, the AAAA records for a few minor domains are not yet in DNS. But I should have that done in the next day or so.

Today I finished migrating my mail server from qmail to Postfix. I had been using qmail for many years, but the fact is it's essentially abandoned as software. And it's not IPv6 enabled. Switching to Postfix took some work (I've got some odd mail setups, mailing lists, etc), but I believe I am done now. The first non-test IPv6 email came in this morning at about 9AM. It was a lottery scam. Ho hum.

The DNS server is listening on IPv6. The [2ping](http://www.finnie.org/software/2ping/) listener on colobox was of course designed to be IPv6 aware from the start. And I am still pretty sure I am running [the only IPv6 enabled TCPMUX server on the Internet](http://www.finnie.org/2010/04/30/ipv6-living-in-the-future-etc-etc/).

At this point, everything on the server itself works with IPv6. However, if IPv4 simply disappeared tomorrow, there is one crucial link that would prevent anyone from getting to it. Only one of the DNS servers (the primary on the box itself) is IPv6 enabled, but IPv6 glue records for it (feh.colobox.com) are not published to the .com zone. DNS glue solves the chicken-and-egg problem of name-based delegation of names.

One small problem though. Since 1999, I've been using Dotster for domain registration. Dotster was one of the first competing registrars opened after the Internic monopoly was broken up in the late 90s, and for the most part I've been happy with them. They're not the cheapest, but they're stable. However, they don't offer the ability to publish IPv6 glue. And DNS glue can only be published by the registrar for the domain you're publishing.

So tonight I sat down and began the process of transferring colobox.com to Gandi. Personally, this is the first time I've transferred a domain in almost 12 years, and the second overall. I registered finnie.org through Internic in 1997, and transferred it to Dotster in 1999.

Though professionally, I've transferred hundreds of domains back and forth over the years. My employer is an OpenSRS reseller registrar. We're not primarily a registrar, but work with enough client sites that becoming a sub-registrar made sense. So I'm no stranger to the process. Generally transfers take 5-21 days, with 7 being about average. After it's in Gandi's hands, I can push the DNS glue and claim 100% IPv6 compatibility.

So that's the status of the colo box. At home, I have Charter, and they have signaled nothing in regard to offering IPv6 so far. So in the meantime I use tunnelbroker.net as an IPv6 proxy. Most equipment at home is IPv6 enabled. (Linux, Windows and Mac OS X with full control. iPhone and Apple TV, but they only take automatic IPv6 router advertisements; basically DHCP but with less functionality.) Some aren't, such as the Xbox 360, Blu-ray player and satellite DVR.

At the office, I emailed our account rep today on a whim, and he said they've actually had IPv6 transit for a year now. We're going to talk next week about going forward with addressing.
