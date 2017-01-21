---
date: 2010-01-16 16:17:54-08:00
layout: post
title: oui, an IEEE OUI database search utility
wp_id: 1240
---
On Friday, I was grepping through DHCP logs, looking for a certain machine, and got sick of going to the IEEE web site to plug in the OUI to figure out the manufacturer of MAC addresses. This involved manually converting a "standard" format MAC address (00:04:f2:e6:93:16, for example) into an OUI format that would be accepted by the IEEE site (00-04-F2).

I found that my workstation already had several OUI databases installed locally (most notably one provided by the nmap package), and hacked together a 5 line Perl script to take a MAC address and use it to search one of the OUI databases. I later fleshed it out into a complete, releasable product. You can download the program from <http://www.finnie.org/software/oui/>.

At its simplest, give it a MAC address, and it will return a vendor.

<pre>$ oui 00:22:19:df:a8:2b
002219 Dell</pre>

You can give it multiple items to look up. These can be either a full MAC address or just an OUI, uppercase or lowercase, and can be in a variety of popular formats.

<pre>$ oui 00:30:48:88:1B:AF 0004f2e69316 000a.4137.c40a 00:26:99:8d:38:ea 00-50-8D
0004F2 Polycom
000A41 Cisco Systems
003048 Supermicro Computer
00508D Abit Computer</pre>

Note that oui was given 5 items, but only returned 4 results. oui searches for several common OUI databases that may be installed locally on your system (the most common would be nmap's database), and they can be quite out of date. Let's rectify that by downloading a current database from the IEEE.

<pre>$ oui 00:26:99:8d:38:ea
$ wget -O /tmp/oui-20100116.txt http://standards.ieee.org/regauth/oui/oui.txt
--2010-01-16 15:04:12--  http://standards.ieee.org/regauth/oui/oui.txt
Resolving standards.ieee.org... 140.98.193.16
Connecting to standards.ieee.org|140.98.193.16|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2118408 (2.0M) [text/plain]
Saving to: `/tmp/oui-20100116.txt'

100%[=====================================&gt;] 2,118,408   1019K/s   in 2.0s

2010-01-16 15:04:17 (1019 KB/s) - `/tmp/oui-20100116.txt' saved [2118408/2118408]

$ oui -d /tmp/oui-20100116.txt 00:26:99:8d:38:ea
002699 Cisco Systems</pre>

Much better. If you want to permanently store that database, put it in `/usr/share/oui/oui.txt`; oui will look there first for a database.

You can also use oui to search one or more organization names.

<pre>$ oui -s avaya "university of california"
00040D Avaya
00126D University of California, Berkeley
001B4F Avaya
00E007 Avaya ECS</pre>

Let's take a look at how many registrations some companies have.

<pre>$ oui -s dell | wc -l
29</pre>

That's about 500 million possible MAC addresses, which sounds right for the world's largest PC manufacturer. Let's try my favorite server manufacturer, Supermicro.

<pre>oui -s supermicro "super micro" | wc -l
2</pre>

Ahh, not so much. What about Cisco? They seem to have a lot of devices out there on the ol' Internets.

<pre>$ oui -s cisco | wc -l
448</pre>

Wow. That's 7.5 billion possible MAC addresses.

We can also see how many registrations are currently marked "private". These are registrations where the IEEE keeps the manufacturer's identity private for a time, in exchange for a yearly fee.

<pre>$ oui -s -c '^PRIVATE$' | wc -l
43</pre>

A few notes: First, you will get no results if you just have the nmap database installed, as it uses a condensed format and filters out private registrations. Second, you can use regular expressions to match an organization (PCRE). Third, the -c flag forces the search to be case sensitive.
