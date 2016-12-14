---
date: 2012-08-22 00:04:01-07:00
layout: post
tags:
- planetcanonical
title: Introducing apache-vsl
wp_id: 2028
---
I'm not a programmer, honest, but lately I've had this annoying habit of releasing code. But instead of my normal rambling posts about new announcements, I'll try to keep it brief.

[apache-vsl](http://www.finnie.org/software/apache-vsl/) has been one of my "90/90" projects (90% done, just need to finish the remaining 90%) for many years; I believe I started it around 2006, and I finally got the urge to finish and release it to the public. It's an Apache logging daemon (managed by Apache itself), capable of intelligently handling multiple VirtualHosts while being as efficient and extensible as possible. It's specifically built for two use cases -- servers which have a large number of VirtualHosts (hundreds or more), and VirtualHosts which receive a lot of traffic -- but it's easy enough to manage that it's useful with any Apache installation.

You can define time-based log files (monthly, daily, hourly... it's all just strftime), and apache-vsl handles when to log to a new file according to your definition. It manages symlinks to both the current and previous logfiles (for example, <tt>access_log.2012-08 -> access_log</tt> and <tt>access_log.2012-07 -> access_log.old</tt>), and has support for program triggers when a rotation happens (say, for running Webalizer against the old logfile, or compressing it).

apache-vsl manages its open files efficiently, so for a server with many VirtualHosts (not all of which may be accessed very often), all of the VirtualHosts' log files won't be open at once, but for VirtualHosts which are accessed frequently, the log filehandle will not constantly be opening and closing.

apache-vsl shares many features with [cronolog](http://cronolog.org/), but while cronolog is designed more for splitting logfiles after the fact (if you were to use it directly from Apache, you'd need to have Apache open a dedicated pipe for every VirtualHost), apache-vsl is designed to communicate with Apache with a single pipe for multiple VirtualHosts.

A lot of good documentation is available in [the manpage](http://www.finnie.org/software/apache-vsl/apache-vsl.8.html), so if you're interested, I suggest you start there.
