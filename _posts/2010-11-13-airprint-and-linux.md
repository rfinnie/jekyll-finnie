---
id: 1634
title: AirPrint and Linux
date: 2010-11-13T00:58:58+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/?p=1634
permalink: /2010/11/13/airprint-and-linux/
openid_comments:
  - 'a:1:{i:0;s:6:"198357";}'
categories:
  - Uncategorized
tags:
  - planet:canonical
---
[<img src="/blog-media/2010/11/airprint-linux-1-200x300.png" alt="" title="airprint-linux-1" width="200" height="300" class="size-medium wp-image-1639" srcset="/blog-media/2010/11/airprint-linux-1-200x300.png 200w, /blog-media/2010/11/airprint-linux-1.png 640w" sizes="(max-width: 200px) 100vw, 200px" />](/blog-media/2010/11/airprint-linux-1.png) [<img src="/blog-media/2010/11/airprint-linux-2-200x300.png" alt="" title="airprint-linux-2" width="200" height="300" class="size-medium wp-image-1640" srcset="/blog-media/2010/11/airprint-linux-2-200x300.png 200w, /blog-media/2010/11/airprint-linux-2.png 640w" sizes="(max-width: 200px) 100vw, 200px" />](/blog-media/2010/11/airprint-linux-2.png)

I've been trying the iPhone OS<sup>[1]</sup> 4.2 GM, and decided to play with the AirPrint functionality. However, Apple has removed the ability to print to locally attached PC/Mac printers with the final versions of iTunes 10.1 and OS X 10.6.5. Using a beta version of iTunes 10.1 for Windows, Wireshark, avahi-discover and a few other tools, I managed to get an understanding of how AirPrint works. What I've found is:

  * AirPrint uses IPP for print management.
  * AirPrint listens to mDNS (Bonjour/Avahi) for printer discovery.
  * AirPrint requires a \_universal subtype to be present in the \_ipp announcement before it will consider listing the printer.
  * AirPrint requires an additional TXT record, "URF", to be present and non-empty before it will consider listing the printer.
  * While this URF format (see below) appears to be a future option for Apple, all current AirPrint-enabled apps seem to send print data as PDF.
  * When a printer is protected by a username/password, the iTunes/AirPrint daemon will send a TXT record "air=username,password".

The original announcement as sent by iTunes 10.1 Beta 2 for Windows looked mostly like a normal IPP announcement, except for the _universal subtype (which standard CUPS does not seem to send), and the following TXT records:

<pre>pdl=application/pdf,image/jpeg,image/urf
URF=W8,DM1,CP255,RS600-300
air=username,password</pre>

(The last record is a literal string "username,password", not an obfuscated user/pass.)

I haven't been able to find much info about what "URF" is. I'm guessing a raster format of some kind. The CUPS implementation in the iTunes beta is accepting it though. What I found is that the image/urf type is not required to be in the pdl record. Currently, all AirPrint enabled apps seem to be fine with sending PDFs, so as long as your CUPS daemon has a PDF filter, you're good. What IS required is the URF record. It has to exist, and cannot be empty, or the printer does not show up in the AirPrint list. The "air" record seems to exist to provide a hint to AirPrint that the target requires a login. If your printer target is not protected by a login, this record can be omitted.

So, with this known, how do we set up a Linux (or presumably *BSD) server as an AirPrint server? We're going to have to manually set up an Avahi service. CUPS can publish to Avahi, but as this requires an extra record and subtype that CUPS does not provide, we'll have to duplicate the printer record manually.

Note that most of this is Debian-centric. If you are not running Debian, you'll probably have to adjust things here and there.

1. Install CUPS and make sure it's actually working. Configuring CUPS correctly is outside the scope of this guide. You will need to have a PDF filter working correctly. Debian seems to provide this out of the box with a CUPS install; your mileage may vary.

2. Go into the CUPS interface and make sure "Share published printers connected to this system" is enabled.

3. Install avahi-daemon and make sure it is running. Restart CUPS to get it to publish printer services. (AirPrint will not be using the CUPS-published services, but we will need them to recreate a manual service.)

4. On another Linux box, install avahi-discover and run it from a terminal. Click the printer you wish to duplicate. Now switch back to the console and a set of debugging lines should be printed similar to this (I've added some linebreaks for readability):

<pre>Service data for service 'HP LaserJet 1200 @ nibbler.xn--n3h' of type '_ipp._tcp' in domain 'local' on 3.0:
    Host nibbler.local (10.9.8.1), port 631, TXT data: [
        'pdl=application/pdf,application/postscript,application/vnd.cups-raster,application/octet-stream,image/png',
        'Copies=T',
        'Duplex=T',
        'Binary=T',
        'Transparent=T',
        'printer-type=0x3056',
        'printer-state=3',
        'product=(GPL Ghostscript)',
        'note=Home',
        'ty=HP LaserJet Series PCL 4/5, 1.3',
        'rp=printers/hp1200',
        'qtotal=1',
        'txtvers=1'
    ]</pre>

5. Back on the server, create <tt>/etc/avahi/services/<em>name</em>.service</tt>. _name_ can be anything; I named it <tt>/etc/avahi/services/hp1200.service</tt> in this case.

6. Create an XML file similar to the example below, using the data above as the values. Note that the data is in the opposite order of what avahi-discover gave it to you.

<pre>&lt;?xml version="1.0" standalone='no'?&gt;
&lt;!DOCTYPE service-group SYSTEM "avahi-service.dtd"&gt;
&lt;service-group&gt;
&lt;name replace-wildcards="yes"&gt;AirPrint hp1200 @ %h&lt;/name&gt;
&lt;service&gt;
       &lt;type&gt;_ipp._tcp&lt;/type&gt;
       &lt;subtype&gt;_universal._sub._ipp._tcp&lt;/subtype&gt;
       &lt;port&gt;631&lt;/port&gt;
       &lt;txt-record&gt;txtvers=1&lt;/txt-record&gt;
       &lt;txt-record&gt;qtotal=1&lt;/txt-record&gt;
       &lt;txt-record&gt;rp=printers/hp1200&lt;/txt-record&gt;
       &lt;txt-record&gt;ty=HP LaserJet Series PCL 4/5, 1.3&lt;/txt-record&gt;
       &lt;txt-record&gt;note=Home&lt;/txt-record&gt;
       &lt;txt-record&gt;product=(GPL Ghostscript)&lt;/txt-record&gt;
       &lt;txt-record&gt;printer-state=3&lt;/txt-record&gt;
       &lt;txt-record&gt;printer-type=0x3056&lt;/txt-record&gt;
       &lt;txt-record&gt;Transparent=T&lt;/txt-record&gt;
       &lt;txt-record&gt;Binary=T&lt;/txt-record&gt;
       &lt;txt-record&gt;Duplex=T&lt;/txt-record&gt;
       &lt;txt-record&gt;Copies=T&lt;/txt-record&gt;
       &lt;txt-record&gt;pdl=application/pdf,application/postscript,application/vnd.cups-raster,application/octet-stream,image/png&lt;/txt-record&gt;
       &lt;txt-record&gt;URF=none&lt;/txt-record&gt;
&lt;/service&gt;
&lt;/service-group&gt;</pre>

The only differences are 1) I named the service "AirPrint hp1200" to not interfere with the "hp1200" service that CUPS publishes, and 2) the added "URF=none" record. And be sure to double check that "application/pdf" is included in the pdl record. If it's not there, don't simply try to add it, that won't work. You'll need to figure out why your CUPS installation doesn't have a PDF filter.

7. Save the file. avahi-daemon should notice the new file and load it without a needed restart.

8. On your iPhone/iPad, go into an application (Safari, for example), print, and search for printers. Your new printer definition should be in there.

9. Print!

10. (Optional) Wonder why you're still printing things in 2010, let alone from a mobile device. For me, this was mostly an exercise in figuring out how AirPrint worked. The example printer used here, an HP LaserJet 1200, was bought used as a demo/display model from an office supply store in 2002. I print maybe 10 pages per year on it. 8 years later, and the original demo toner cartridge is still working fine.

--

<sup>[1]</sup> I've been working with Cisco IOS for over 10 years now, and flat out refuse to call this "iOS". Live with it.
