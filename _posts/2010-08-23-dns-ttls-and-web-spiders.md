---
date: 2010-08-23 00:39:54-07:00
layout: post
title: DNS TTLs and web spiders
wp_id: 1495
---
I've moved a few servers at work to our new datacenter, using the "normal" method: adjust TTLs from 3600 to 120, wait an hour, sync servers, change DNS to new IP, all traffic should be going to new server within 2 minutes, bump TTLs back up to 3600.

Google and Yahoo seem to be compliant, but there are invariably the same 3 bots that appear on the old server up to a day after the switch:

  * Baiduspider+(+http://www.baidu.com/search/spider.htm)
  * msnbot/2.0b (+http://search.msn.com/msnbot.htm)
  * Mozilla/5.0 (compatible; YandexBot/3.0; +http://yandex.com/bots)

Maybe there should be an option in robots.txt.

<pre>User-agent: *
Honor-DNS-TTLs: no-really-you-should-be-doing-this</pre>
