---
id: 1254
title: 'in.tcpmuxd: A secure, RFC compliant TCPMUX server'
date: 2010-02-13T03:41:49+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/?p=1254
permalink: /2010/02/13/in-tcpmuxd-a-secure-rfc-compliant-tcpmux-server/
openid_comments:
  - 'a:1:{i:0;s:5:"18778";}'
categories:
  - Uncategorized
---
Yesterday, on IRC, [neale](http://woozle.org/~neale/) asked if it was wise to run a TCP service on port 1. [sneakums](http://zork.net/~sneakums/) replied it was not, since it was a registered service, "tcpmux". However, nobody immediately knew what "tcpmux" was; [Wikipedia provided the answer](http://en.wikipedia.org/wiki/TCPMUX).

TCPMUX is an ancient, horrible protocol. You connect to a TCPMUX server on port 1, then tell it which TCP service you actually wanted, and it forwards locally for you. Obviously fraught with security problems on the modern Internet. Nonetheless, I immediately wanted to write a TCPMUX server.

I started out by coding to the description in the Wikipedia entry, not knowing there was an RFC. We did find it ([RFC 1078](http://www.faqs.org/rfcs/rfc1078.html)), and Neale and I went back and forth tweaking the code. Eventually I stopped with this:

> <pre>#!/usr/bin/perl

while(&lt;>) {
  if($_ eq "HELP\r\n") {
    print "tcpmux\r\n";
    exit 0;
  } elsif(lc($_) eq "tcpmux\r\n") {
    print "+OK FINE\r\n";
  } else {
    print "-BLOW ME\r\n";
    exit 0;
  }
}</pre>

My friends, that is a fully functional, RFC 1078-compliant, completely secure TCPMUX server, in 11 lines of Perl. Neale has a bash version that he prefers, but I argue mine is better because it's strictly RFC-compliant (only accepts CRLF, etc). To use it, add this to <tt>/etc/inetd.conf</tt>:

> <pre>tcpmux stream tcp nowait nobody /path/to/in.tcpmuxd</pre>

To use, telnet to port 1. (You can use <tt>nc</tt>, but you will have to do something like "<tt>echo -ne 'tcpmux\r\n' | nc localhost 1</tt>" because it will only recognize CRLF-terminated lines per the RFC.) in.tcpmuxd will accept and forward exactly one service, tcpmux. All others will be rejected with a kind explanation. "HELP" will also conveniently list all services it will forward.

You can also test this by telnetting to <tt>colobox.com</tt> port 1, which is running a fully functional TCPMUX server.

This service has been painstakingly checked for security flaws. A highly skilled team has gone through the entire codebase, line by line, and has determined that there are no known implementation or security flaws in the service. You're welcome.