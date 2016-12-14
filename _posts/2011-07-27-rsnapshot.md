---
id: 1859
title: rsnapshot
date: 2011-07-27T21:59:27+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/?p=1859
permalink: /2011/07/27/rsnapshot/
categories:
  - Uncategorized
tags:
  - planet:canonical
---
At my former employer, we had an in-house backup system for backing up Unix servers. It was called **speede**, and it offered a much better way of maintaining disaster recovery backups than other methods such as tape. Multiple snapshots were taken, by taking the old snapshot, coping it to the new snapshot using hardlinks, and running rsync from the backed up host to the new snapshot. Since rsync works (by default) by copying changed files to a temp file, deleting the old destination file and moving the temp file into place, it cleanly breaks the hardlink without affecting the data in the old inode. The net result is you have multiple snapshot directories that look like completely independent directory trees, but are space efficient since the majority of the files share the same inodes between snapshots. If you have 200MB of data in snapshot 1 and 5MB of files change between snapshot 1 and snapshot 2, only 205MB is stored on disk.

(Apple uses the same type of process for Time Machine, by the way.)

speede was started sometime in 2002. I started at the company in 2004, and took over development of it in early 2005. Over the next 6 years, a lot was added, such as run concurrency, more granular options, etc. It was an awesome system. We even planned on releasing it in 2009 as open source, but company politics put an end to that in the 11th hour (we were in the process of getting acquired by another company at the time).

After that, I decided to do a semi-cleanroom re-coding of it and instead releasing that, calling it **bahlgs** ("Backups Are Hard, Let's Go Shopping!"). Unfortunately, that became one of my "90% done, just need to finish the other 90%" projects, and it was never released.

I had heard about **[rsnapshot](http://rsnapshot.org/)** awhile ago, which did nearly the same thing as speede. But I never looked much into it, since we had an elegant system that did exactly what we needed.

I ran an early dev copy of bahlgs on my home router / server, backing up a few home servers, my colo box, my Linode, etc. Today I upgraded my home server, using it as an excuse to reinstall the OS at the same time. Rather than setting up that copy of bahlgs again, I decided to take a better look at rsnapshot.

It seems to be pretty decent, and would be capable of functioning as a replacement for my home backup system. But at the same time I was thinking back to my previous employer, where I maintained a datacenter of approximately 150 servers. And I realized that rsnapshot wouldn't have worked for that use:

> speede created snapshot directories in the format <tt>snapshot-YYYYMMDD-HHMM</tt>, with a symlink from <tt>current</tt> to the latest snapshot. rsnapshot uses a format like <tt>daily.0</tt>, where "daily" is the type of snapshot and "0" is an incrementing integer, with 0 always being the most current. That avoids the need for a "current" symlink, but makes it harder to see what dates correspond to which snapshots.
> 
> speede had hardcoded logic for the last N daily backups, plus the first snapshot of the month. It's something I wanted to make more configurable, but it served our backup needs. rsnapshot allows for a configurable number of hourly/daily/weekly/monthly snapshots, which is more configurable, but the runs are not connected to each other. That is, if I choose to do daily and monthly snapshots, the backup is run twice on the day the monthly snapshots are run.
> 
> speede had concurrency support, allowing for a maximum number of concurrent rsyncs (6 by default), and hosts could be placed into concurrency groups with separate concurrency limits for them. For example, limit group "vmhost5" to 2 concurrent rsyncs, since if all 6 runs happened to be against guests of VM Host #5, it would impact all guests on the host.
> 
> rsnapshot seems to have no concurrency support. That would be a killer for my old employer, where we had three backup servers, each running 6 concurrent rsyncs, backing up about 150 servers, and it would still take about 8 hours each night. This can be partially mitigated in rsnapshot by using multiple configuration files and dividing the servers up into multiple cron runs done concurrently, but speede was smarter and used a queue system so one large backup wouldn't hold up others.

I'm definitely not putting down rsnapshot. It seems very useful, and even has features that would have been nice for speede, except I never got around to coding them into speede myself (such as pre/post per-backup scripts). But again, I'm not lamenting not taking a look at rsnapshot when I was with my old employer, since the lack of concurrency support would have been a deal breaker.

Both speede and rsnapshot are coded in Perl, so I may look into adding the "missing" functionality myself, and submitting it upstream. But for now, I think I'll install rsnapshot at home.
