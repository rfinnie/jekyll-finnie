---
author: Ryan Finnie
categories:
- Uncategorized
date: 2015-07-23 20:53:41
excerpt: dsari is a lightweight continuous integration (CI) system. It provides scheduling,
  concurrency management and trigger capabilities, and is easy to configure. Job scheduling
  is handled via dsari-daemon, while dsari-render may be used to format job run information
  as HTML.
guid: http://www.finnie.org/2015/07/23/dsari-do-something-and-record-it/
id: 2469
layout: post
permalink: /2015/07/23/dsari-do-something-and-record-it/
title: dsari - Do Something and Record It
---
When [Finnix](http://www.finnix.org/) first started transitioning to [Project NEALE](http://www.finnix.org/Project_NEALE), the ability to produce Finnix builds from scratch in a normalized fashion, I began making NEALE builds on a nightly schedule using cron. This was fine in the beginning, but as things got more complex (there are currently [16 different variants](http://ci.finnix.org/dsari/) of Finnix being built nightly), I started looking at alternatives.

Like many people throughout history, my ad-hoc CI system transitioned from a cron script to Jenkins. This worked decently, but there were drawbacks. Jenkins requires Java, and is very memory intensive. I had ARM builders being fed by Jenkins, and found the remote was taking up most of the memory. Occasionally remotes would just freeze up. And since the main instance was running inside my home network, I needed to proxy the web interface from my main colo box for reports to be visible to the world. Overall, Jenkins had the feel of a Very Big Project, complete with drawbacks.

That got me thinking of what I would need for something midway between cron and Jenkins. "Well, I basically need to do something and record it. Everything else can hang off the 'do something' part, or the 'record it' part." And with that, [dsari](https://github.com/rfinnie/dsari) was born.

[dsari](https://github.com/rfinnie/dsari) is a lightweight continuous integration (CI) system. It provides scheduling, concurrency management and trigger capabilities, and is easy to configure. Job scheduling is handled via `dsari-daemon`, while `dsari-render` may be used to format job run information as HTML.

That's basically it. All other functionality is based on the idea that you have a better idea of what you want to do than I do. The "do something" portion of the job run is literally running a single command - this is almost always a shell script. For example, all of the jobs used to do NEALE builds call the same shell script, which uses the `JOB_NAME` and `RUN_ID` environment variables to determine what variants to build. The shell script then performs the build and [emails me](https://github.com/rfinnie/dsari/blob/master/doc/notifications.md) if a run fails or returns to normal.

Want to produce an off-schedule run [based on a trigger event](https://github.com/rfinnie/dsari/blob/master/doc/triggers.md), such as a VCS commit? dsari has a powerful trigger system, but it's based on the idea that you figure out what the trigger event is, and you write the trigger configuration file which dsari picks up on.

dsari has a [decent scheduler](https://github.com/rfinnie/dsari/blob/master/doc/schedule-format.md) which is based off the cron format, with Jenkins-style hash expansion so you can easily spread runs out without having to hard-code separation. And dsari has an [expansive concurrency system](https://github.com/rfinnie/dsari/blob/master/doc/concurrency.md) which lets you limit runs to one or more concurrency groups, which lets you do things like resource limiting and/or pooling.

Run data (output and metadata) is stored in a standardized location, and dsari includes a utility which renders the data as [simple HTML reports](http://ci.finnix.org/dsari/). You may then sync the HTML tree to the final destination, rather than relying on exposing a web daemon.

dsari fits my requirements: a simple CI system which slots somewhere between cron and Jenkins. Surely this will be insufficient for some people, while it will be overkill for others. Hopefully it will be useful to people.
