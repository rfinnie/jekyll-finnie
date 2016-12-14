---
id: 2072
title: 2ping 2.0 released
date: 2012-04-22T20:14:45+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/2012/04/22/2ping-2-0-released/
permalink: /2012/04/22/2ping-2-0-released/
categories:
  - Uncategorized
tags:
  - 2ping
  - planet:canonical
---
[2ping 2.0](http://www.finnie.org/software/2ping/) has been released today. User-visible changes are minor, but behind the scenes, a major update to the protocol specification has been implemented, justifying the major version bump:

  * Updated to support [2ping protocol 2.0](http://www.finnie.org/software/2ping/2pingprotocol2.0-20120422.pdf) 
      * Protocol 1.0 and 2.0 are backwards and forwards compatible with each other
      * Added support for extended segments
      * Added extended segment support for program version and notice text
      * Changed default minimum packet size from 64 to 128 bytes
  * Added peer reply packet size matching support, turned on by default
  * Added extra error output for socket errors (such as hostname not found)
  * Added extra version support for downstream distributions
  * Removed generation of 2ping6 symlinks at "make all" time (symlinks are still generated during "make install" in the destination tree)

2ping is a bi-directional ping utility. It uses 3-way pings (akin to TCP SYN, SYN/ACK, ACK) and after-the-fact state comparison between a 2ping listener and a 2ping client to determine which direction packet loss occurs.
