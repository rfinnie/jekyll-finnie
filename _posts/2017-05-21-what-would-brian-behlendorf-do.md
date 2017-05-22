---
date: 2017-05-21 23:06:01-07:00
excerpt: Wherein Ryan resurrects even more of the 90s.
excerpt_standalone: true
layout: post
title: What would Brian Behlendorf do?
---
(With apologies to both Brian Behlendorf and [Mr. Bad](http://zork.net/pipermail/crackmonkey/2000-September/013232.html) for the title.)

In 1996, the final official release of [NCSA HTTPd](https://en.wikipedia.org/wiki/NCSA_HTTPd) was made, 1.5.2a.  If you were born after 1996, just remember that there were web servers which came before [Apache](https://en.wikipedia.org/wiki/Apache_HTTP_Server).  Also, get off my lawn.

In 2007, I decided on a lark to get ncsa-httpd working on (at the time) modern Linux/GCC.  It only took a handful of changes, and I packaged it up as an (unpublished) Debian package, ignoring modern FHS requirements and continuing to put everything under `/usr/local/etc/httpd/`, as it was in the 90s.

This evening, my IRC friend Screwtape posted a link he found about [building a new Gopher search engine using 1990s technology](https://blog.benjojo.co.uk/post/building-a-search-engine-for-gopher), and that got me thinking about ncsa-httpd again.  I went back to my project from 2007 and discovered it was no longer buildable on GCC 5, and with help from Screwtape, I got it building again.

```
HTTP/1.0 200 Document follows
Date: Mon, 22 May 2017 05:56:08 GMT
Server: NCSA/1.5.2
Content-type: text/html
```

After that, I continued on, finding a few bugs in the source code, fixing them, finding a few more, etc.  After a few hours I stopped and realized what I was doing, and threw those patches out.  I was literally reinventing the Apache project, 22 years too late.

I've made the patches required to build on modern systems available in [this `flying-cars` branch](https://github.com/rfinnie/ncsa-httpd/commits/flying-cars).  They consist of a single commit for the fixes I made back in 2007 (which I may split out into individual commits later), as well as the individual fixes made this evening.  And if you're interesting in the Debian packages, [they're available as well](https://www.finnie.org/software/ruins/ncsa-httpd/debian/).