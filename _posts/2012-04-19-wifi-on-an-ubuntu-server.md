---
date: 2012-04-19 19:25:26-07:00
layout: post
tags:
- planetcanonical
title: WiFi on an Ubuntu server
wp_id: 2065
---
One night last week, my cable internet service went down. OK, it rarely happens, and when it does, I have an iPhone with tethering. No big deal normally, I would just have my laptop connect to the tether. However, that night I was in the middle of a few things on my home LAN, and wanted my whole network to have internet access, not just my laptop. Getting wireless access on [my home router/server](http://www.finnie.org/2012/03/09/new-home-server/) seemed the most preferable. So I dug through the junk crates and found:

  * Two Orinoco Gold 802.11b PCMCIA cards
  * Four Orinoco Silver 802.11b PCMCIA cards
  * A Linksys 802.11b USB dongle which required a USB A to A cable
  * A Linksys 802.11g PCI card
  * About a dozen WRT54G (or similar) router/APs

I immediately threw away the USB dongle due to its odd requirement. I should throw away the Orinoco cards as well, but dammit, they had real value (ten years ago). The PCI card would have worked, except my new router/server actually has no "legacy" PCI slots, just PCI Express slots. I could have cobbled together some sort of bridge with a WRT54G, but it would have been too much work. In the end I toughed it out and waited for the cable internet to return.

The next day I ordered a $17 [PCI express 802.11n adapter](http://www.newegg.com/Product/Product.aspx?Item=N82E16833166063), and it arrived today. Ubuntu 12.04 "precise" (currently in beta, set to be released next week) saw it immediately, and with a little reading I was able to get it working with my phone. I'm leaving the configs here in case they are useful to someone else:

<tt>/etc/network/interfaces</tt>:

<pre>#auto wlan0
iface wlan0 inet dhcp
    wpa-conf /etc/wpa_supplicant.conf</pre>

"auto wlan0" is commented out because I obviously don't want it running on boot, since this is a backup solution. That line could simply be omitted, but I like it there to point out to myself it is not brought up on boot.

<tt>/etc/wpa_supplicant.conf</tt>:

<pre>network={
    ssid="Ryan’s iPhone"
    scan_ssid=1
    key_mgmt=WPA-PSK
    psk="yourkeyhere"
}</pre>

iPhone-specific note: that "’" is actually a unicode character, U+2019. "iwlist wlan0 scan" was returning this, which wpa_supplicant was not accepting:

<pre>ESSID:"Ryan\xE2\x80\x99s iPhone"</pre>

In the end, I hopped on my laptop, associated with the phone and grabbed the SSID from the logs and pasted it in on the server, to preserve the unicode character.

With that in place, "ifup wlan0" worked fine. In the future, during an outage I should just be able to log into the server, run "ifdown eth1", then "ifup wlan0", and everything at home should be run through my phone.
