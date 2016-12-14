---
author: Ryan Finnie
date: 2009-08-04 11:23:19
guid: http://www.finnie.org/2009/08/04/civilized-feed-readers/
id: 994
layout: post
permalink: /2009/08/04/civilized-feed-readers/
title: Civilized feed readers
---
I happened to be tailing the logs for finnie.org, and noticed this:

<tt>208.93.0.128 - - [04/Aug/2009:11:07:41 -0700] "GET /feed/ HTTP/1.1" 200 40498 "-" "LiveJournal.com (webmaster@livejournal.com; for http://www.livejournal.com/users/rfinnie/; 7 readers)"</tt>

That's pretty nice of LiveJournal. The bot identifies itself, says what the feed is being used for, and how many users are reading it. No gzip support though, sadly. Upon further examination, Google Reader does something similar:

<tt>209.85.238.147 - - [02/Aug/2009:20:24:57 -0700] "GET /feed/ HTTP/1.1" 304 84 "-" "Feedfetcher-Google; (+http://www.google.com/feedfetcher.html; 4 subscribers; feed-id=12499587138938146489)"</tt>

Google Reader seems to take it one step further, and uses If-Modified-Since (as evidenced by the 304 status code) so it's not constantly pulling the same data. I went back and looked for the last time it actually pulled data:

<tt>209.85.238.147 - - [02/Aug/2009:10:06:07 -0700] "GET /feed/ HTTP/1.1" 200 14209 "-" "Feedfetcher-Google; (+http://www.google.com/feedfetcher.html; 4 subscribers; feed-id=12499587138938146489)"</tt>

14k! gzip! Truly, we have now entered the 20th century.
