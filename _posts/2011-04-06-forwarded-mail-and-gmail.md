---
id: 1788
title: Forwarded mail and Gmail
date: 2011-04-06T17:22:46+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/2011/04/06/forwarded-mail-and-gmail/
permalink: /2011/04/06/forwarded-mail-and-gmail/
categories:
  - Uncategorized
tags:
  - planet:canonical
---
I forward my mail from my personal server to Gmail. For the most part, it would work okay, but [SPF](http://en.wikipedia.org/wiki/Sender_Policy_Framework) is a problem. Specifically, SPF completely breaks legitimate email routing if "properly" configured. And since we're talking about the people emailing you being the ones "properly" configured, there's little you can do about it. (Have I mentioned how much I hate SPF?) My mail server receives mail, then routes it to Gmail. The problem is when the original sender's SPF policy is set to strict, Gmail receives it, looks up the SPF policy, notices it's coming from my server instead of the permitted sender server, and marks it as spam.

In hindsight, the creators of SPF "planned" for this, and created [SRS](http://en.wikipedia.org/wiki/Sender_Rewriting_Scheme), which effectively rewrites the SMTP Envelope FROM (but not From:), from, for example, user@sender.tld to SRS0=ABCD=EF=sender.tld=user@relay.tld.

I implemented this about a month ago on my mail server, and things seemed to be going well at first. But over time, Gmail started marking more and more legitimate mail as spam. Probably due to their servers not being aware of SRS, and seeing a single domain (relay.tld) sending a bunching of mail with From: addresses like facebook.com, etc. In the end, nearly 100% of mail was being marked as spam, so I switched back to simple relaying.

Now, what many email providers do is support a procedure with a name like "inbound gateway", "trusted relay", etc, where you specify which IP addresses are permitted to relay mail to the final destination. This isn't a "whitelist", it basically tells the mail server, "for mail coming from this IP address, do not consider the connecting IP address at all in relation to spam determination, good or bad". The problem is, only some Google mail services provide this:

Free Gmail: NO
  
Free Google Apps Gmail: NO
  
Paid Google Apps Gmail: YES (from the Apps control panel: Service Settings, Email, Inbound gateway)
  
Postini: YES (but requires a call to Postini, and getting them to approve the IP globally, not just for your service)

So I'm considering switching to the paid Google Apps, just to get this behind me. I like Gmail's interface, and more importantly, I'm almost totally reliant on Google for spam filtering, as no other service seems to do as well (barring the problems described here), considering I've had my primary email address for nearly 15 years, and it receives literally thousands of spam per day.
