---
id: 1873
title: Mobile Rickroll Appliance 6.0 released
date: 2011-08-13T13:56:27+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/2011/08/13/mobile-rickroll-appliance-6-0-released/
permalink: /2011/08/13/mobile-rickroll-appliance-6-0-released/
openid_comments:
  - 'a:1:{i:0;s:6:"145926";}'
categories:
  - Uncategorized
tags:
  - planet:canonical
---
<div style="float: right; margin-left: 1em;">
  <a href="http://www.flickr.com/photos/fo0bar/2366164074/" title="Mobile Rickroll Appliance 6.0 by Ryan Finnie, on Flickr"><img src="http://farm4.static.flickr.com/3228/2366164074_fce0ee2d3b_m.jpg" width="240" height="180" alt="Mobile Rickroll Appliance 6.0" /></a>
</div>

Last week, I attended Defcon 19 in Las Vegas. This year was a pretty good year. It was held at the Rio which was not without problems, but the hotel rooms were nice, the conference space was much larger than any previous year, and overall it was a much better experience that the Riviera from the last few years.

At the last minute before I left for Vegas, I found and revived the Mobile Rickroll Appliance 6.0. The MRRA 6.0 is something I created in 2008, modifying a WRT54GS running OpenWRT with some custom iptables and dnsmasq configuration to make a self-contained Rickrolling platform. The result was hilarious:

<iframe width="640" height="360" src="https://www.youtube.com/embed/l0C_wEAx6ao" frameborder="0" allowfullscreen></iframe>

(Since then, the sped-up Rickroll video has been replaced with a normal speed one, but otherwise the functionality is exactly the same.)

This year I re-flashed my WRT54GS and took it to Defcon. Before the conference began, I headed to Fry's and picked up a 7.2 Ah 12v battery (the narrow kind used in some UPSes) and various cables and adapters to go from alligator clips on the battery side to the barrel plug on the WRT. The result was a truly mobile (though heavy) Rickroll appliance that could be stored in my backpack.

That Friday morning, I took it to the opening ceremony and talks. At home, the ESSID was "Common Area", but for Defcon I named it "cellshare", to make it look like someone had mobile hotspot enabled on their phone. The MRRA 6.0 is configured so when someone is Rickroll'd, the big front status light will flash for 5 seconds, then turn solid orange, giving me a quick status indicator. This is done by a web bug CGI on the HTML page, which also logs the time since boot (the WRT doesn't have a battery-saved RTC and the MRRA 6.0 doesn't have internet access, so real time is not possible), the HTTP user agent and the HTTP referer (which in this case is what the victim was originally trying to go to).

Over the next 3 hours, I managed to Rickroll over 100 people! Most victims were iPhone users. On the plus side, the way iOS checks for captive portal support, the Rickroll web page will pop up immediately upon association with the AP; they don't even have to open Safari. The downside is the MRRA 6.0 uses FLV, and the iPhone doesn't support Flash, so all they see is a big box and a message saying they've been Rickroll'd. Future releases may use some sort of MP4 solution.

Of the remaining victims, most were laptop users. Most of them were Windows 7 running IE, though a large minority were OS X (you tend to see a lot of Macs at Defcon). Of the sites people tried to visit, cnn.com was the most popular, followed by google.com, facebook.com and apple.com.

Sadly, the battery pack only worked for just over 3 hours. The battery had a capacity of 7.2 Ah @ 12v, and the WRT lists a draw of 1000 mA, so I was expecting a life of at least 7.2 hours (hopefully longer, since I was guessing the 1000 mA figure was max draw, not typical). After 3 hours, the WRT just stopped powering on. The small LED on the barrel plug adapter was still lighting up, but the WRT just would not work unless I was using the wall wart. I suspect it was one of two things. I made no attempt to charge the sealed lead acid battery after buying it from Fry's, so I have no idea what its current capacity was. That possibly combined with a small voltage drop as the battery discharges may have created a voltage that was low enough the WRT didn't like, but still enough to power the barrel plug adapter's status LED. Despite this being Defcon, nobody in our group had a multimeter handy.

Still, I consider the project a success. If you were at any of the first three talks on Friday and got Rickroll'd after associating with "cellshare", I'd like to hear from you!

This weekend I took my original code, modified it for current OpenWRT (the OpenWRT on my WRT was from 2008), re-rendered the Rickroll video to fit on a 4MB WRT device (the original code would only fit on an 8MB WRT -- WRTSL54GS or early WRT54GD -- which are uncommon), and packaged it up. That's right, now you can [build your very own Mobile Rickroll Appliance 6.0](http://www.finnie.org/software/mrra-6.0/)! Download the tarball, extract, and read the README, which will give you all the info you need to get set up.
