---
date: 2010-08-09 21:37:35-07:00
layout: post
title: Flickr mail bounce
wp_id: 1465
---
One of those rare glimpses into the workings of a large public service. Today I sent [a photo](http://www.flickr.com/photos/fo0bar/4878293154/) from my phone to Flickr using their email gateway, and the first time it bounced back:

<pre>Received: by www118.flickr.mud.yahoo.com (Postfix)
	id D7723381B9; Mon,  9 Aug 2010 20:40:33 +0000 (UTC)
Date: Mon,  9 Aug 2010 20:40:33 +0000 (UTC)
From: MAILER-DAEMON@www118.flickr.mud.yahoo.com (Mail Delivery System)
Subject: Undelivered Mail Returned to Sender
To: me@example.com
MIME-Version: 1.0
Content-Type: multipart/report; report-type=delivery-status;
	boundary="93240381BA.1281386433/www118.flickr.mud.yahoo.com"
Message-Id: &lt;20100809204033.D7723381B9@www118.flickr.mud.yahoo.com&gt;

This is a MIME-encapsulated message.

--93240381BA.1281386433/www118.flickr.mud.yahoo.com
Content-Description: Notification
Content-Type: text/plain

This is the Postfix program at host www118.flickr.mud.yahoo.com.

I'm sorry to have to inform you that your message could not
be delivered to one or more recipients. It's attached below.

For further assistance, please send mail to &lt;postmaster&gt;

If you do so, please include this problem report. You can
delete your own text from the attached returned message.

			The Postfix program

&lt;photos@www118.flickr.mud.yahoo.com&gt; (expanded from
    &lt;myflickrpostid@photos.flickr.com&gt;): Command died with status 111:
    "/usr/bin/php -d log_errors=On
    /var/www/html/www.flickr.com/mail_handlers/handler.php 2&gt;&1
    |/usr/local/bin/perl /usr/local/bin/log2spread -h -g error". Command
    output: Could Not Connect

--93240381BA.1281386433/www118.flickr.mud.yahoo.com
Content-Description: Delivery report
Content-Type: message/delivery-status

Reporting-MTA: dns; www118.flickr.mud.yahoo.com
X-Postfix-Queue-ID: 93240381BA
X-Postfix-Sender: rfc822; me@example.com
Arrival-Date: Mon,  9 Aug 2010 20:36:31 +0000 (UTC)

Final-Recipient: rfc822; photos@www118.flickr.mud.yahoo.com
Original-Recipient: rfc822; myflickrpostid@photos.flickr.com
Action: failed
Status: 5.0.0
Diagnostic-Code: X-Postfix; Command died with status 111: "/usr/bin/php -d
    log_errors=On /var/www/html/www.flickr.com/mail_handlers/handler.php 2&gt;&1
    |/usr/local/bin/perl /usr/local/bin/log2spread -h -g error". Command
    output: Could Not Connect

--93240381BA.1281386433/www118.flickr.mud.yahoo.com
Content-Description: Undelivered Message
Content-Type: message/rfc822

[Original (truncated) message here]

--93240381BA.1281386433/www118.flickr.mud.yahoo.com--</pre>
