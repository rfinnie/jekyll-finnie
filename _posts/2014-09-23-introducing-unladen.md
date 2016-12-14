---
id: 2385
title: Introducing Unladen, easily scalable object storage
date: 2014-09-23T22:01:50+00:00
author: Ryan Finnie
excerpt: |
  Unladen is designed to be a cheap, easily scalable object storage system.  "Cheap" in the sense that you can easily use your infrastructure's existing servers' spare space to create (or add to) an ad-hoc cluster.  Or you could build massive dedicated 500TB storage nodes.  Or a combination of those two extremes.
layout: post
guid: http://www.finnie.org/2014/09/23/introducing-unladen/
permalink: /2014/09/23/introducing-unladen/
categories:
  - Uncategorized
---
In 2002, I managed a datacenter's IS infrastructure. We had a few dozen servers, and a terrible backup server which, in theory, backed up servers to tape. In theory, anyway. I've never encountered a commercial backup server which has worked well. Anyway, I thought, "We have these servers, and they all have free space to some extent. I'd like a system which did backups of servers, split them into chunks, encrypted them, and sent multiple copies of them to the other servers." Nothing ever became of that idea at the time.

Fast forward 12 years. Everything is now Yay Cloud, and many are familiar with object storage, likely through Amazon S3. [OpenStack](http://www.openstack.org/) is quickly becoming a large part of many organizations, and OpenStack's object storage component is called [Swift](http://swift.openstack.org/). From a client perspective, Swift is a very nice system. The standalone command-line client is decent, and the API is HTTP and very RESTy: "PUT /v1/account/container/object" to upload an object, DELETE to delete it, etc.

The problem is I've never been very happy administering it. The server software is finicky, and for the most part, it requires a very homogeneous storage infrastructure. You'll want to have storage nodes with the same amount of storage per node, and if you have multiple disks per node, you're required to work out the optimal weight map of the disk compared to the entire cluster. The location of data on the nodes is done via hash rings. In theory this is nice since you can work out the location of an object within the cluster based solely on the ring definition file, but in practice it means a ring configuration which you must (manually) keep up to date on all the storage/proxy nodes, and any sort of change to the cluster (losing or adding a disk or node, etc) means multiple total rebalances of the cluster.

This made me think of my idea from 2002, and the result is (or will be) [Unladen](https://github.com/rfinnie/unladen). Unladen is a Swift client-compatible system which takes a much different approach to the backend.

First, let me say that while I've written a lot of code in the last few weeks and things are looking very usable, this is _nowhere near production quality_. Everything is still very, very pre-alpha, and it seems that every commit I make does things like breaking compatibility with the old schema, etc. While I encourage you to download and play with it, don't trust it with anything more than "Hello World". Oh, and there's no sort of authorization yet, so everyone (including unauthenticated users) have full access.

Unladen is Swift API-compatible, so if you have an application which supports Swift, it'll likely work out of the box with Unladen. But on the backend is where things get more interesting. All object data is encrypted, and is sent to storage nodes that way. Only the trusted catalog nodes have the keys to each object's data, so storage nodes do not have to be trusted.

In addition to data trust, Unladen also has a concept of availability trust, also known as confidence. Say you have a core set of storage nodes, and give a confidence of 100% to each. You can also have nodes which you do not trust to be as available. You can then define replica targets for certain sets of data. The default is a replica target of 3.0, which means an object's (again, encrypted) data would be stored on three 100% confidence nodes, or six 50% confidence nodes, or a combination of them.

Replica targets and confidence determine _how much_ to store, but weight defines _where_ to store it. And weighting is easy with Unladen. Each storage node can have multiple internal "stores", which are just directories. You just tell Unladen how much each store can hold. (In most cases this will simply be mount directories of disks, and "how much" would be ~90% of the disk's filesystem capacity.) When data is placed on a mount, the weight of the stores' sizes determines the balance. Likewise, each node advertises its total storage capability (or a portion thereof), and balancing across the cluster is done according to the automatically-determined weight map of the cluster.

Unladen is designed to be a cheap, easily scalable object storage system. "Cheap" in the sense that you can easily use your infrastructure's existing servers' spare space to create (or add to) an ad-hoc cluster. Or you could build massive dedicated 500TB storage nodes. Or a combination of those two extremes.

Again, this is a very early work in progress. I was a bit hesitant publicly announcing it before it was "ready", but things have been looking good, and I didn't want this to turn into a "90/90" project (90% done, just need to finish the other 90% before I release it, which of course never happens). And the last major project I announced before it was ready was [2ping](http://www.finnie.org/software/2ping/), which turned out to be a great success.

Note that this project is purely a personal endeavor, and is not supported or endorsed by the OpenStack project or my employer.
