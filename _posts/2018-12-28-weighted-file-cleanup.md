---
date: 2018-12-28 15:27:41-08:00
excerpt: Wherein Ryan is paranoid... about disk usage efficiency
excerpt_standalone: true
layout: post
title: Weighted file cleanup
---
I've got four security cameras streaming to my home server, and save the most recent 3 TiB of raw recordings locally. I won't go too much into the details (because they're out to get me, of course), but cam2 and cam4 are the main ones, and I would like to save them locally the longest.  However, cam4's file sizes are larger than cam2's.  cam1 is in an unimportant area so I don't want to save as much, and cam3 is only used on demand, so it has an even lower priority.

I struggled with a system for deleting old recordings, but recently came up with what I believe is a decent system.  I assign file globs into per-camera collections, and then assign a weight to each collection.  cam2 and cam4 each get a weight of 1.0, cam1 gets 0.2 and cam3 gets 0.1.

It then goes through what can be multiple rounds, determining if each collection is over or under its weighted share of the total target.  If a collection is under target, it is eliminated from the round, but importantly its usage is removed from the considered total target for the next round.  Rounds continue until all collections in the round are over target.

```
cam1: 3809 files, 364.82 GiB, 0.20 weight
cam2: 8226 files, 809.57 GiB, 1.00 weight
cam3: 529 files, 83.86 GiB, 0.10 weight
cam4: 7514 files, 1817.40 GiB, 1.00 weight
Grand total: 3075.66 GiB used, 3072.00 GiB target, 3.66 GiB above target
Round 1: cam1: 364.82 GiB used, 267.13 GiB target, 8.70% round weight of 3072.00 GiB, 97.69 GiB above target
Round 1: cam2: 809.57 GiB used, 1335.65 GiB target, 43.48% round weight of 3072.00 GiB, -526.08 GiB above target (disqualifying)
Round 1: cam3: 83.86 GiB used, 133.57 GiB target, 4.35% round weight of 3072.00 GiB, -49.70 GiB above target (disqualifying)
Round 1: cam4: 1817.40 GiB used, 1335.65 GiB target, 43.48% round weight of 3072.00 GiB, 481.75 GiB above target
Round 2: cam1: 364.82 GiB used, 363.09 GiB target, 16.67% round weight of 2178.57 GiB, 1.73 GiB above target
Round 2: cam4: 1817.40 GiB used, 1815.47 GiB target, 83.33% round weight of 2178.57 GiB, 1.93 GiB above target
cam1: Freeing 1.73 GiB
Deleting /media/camera/streams/cam1/cam1_XXX.mp4 (175.85 MiB)
Deleting /media/camera/streams/cam1/cam1_XXX.mp4 (102.45 MiB)
Deleting /media/camera/streams/cam1/cam1_XXX.mp4 (103.56 MiB)
[...]
cam4: Freeing 1.93 GiB
Deleting /media/camera/streams/cam4/cam4_XXX.mp4 (844.88 MiB)
Deleting /media/camera/streams/cam4/cam4_XXX.mp4 (839.17 MiB)
Deleting /media/camera/streams/cam4/cam4_XXX.mp4 (797.06 MiB)
```

In this example, in round 1, both cam1 and cam4 are over their weighted share of the total 3072 GiB target.  cam2 and cam3 are under so they are removed, and the total target of the next round's considered collections is reduced to 2178.57 GiB.  In round 2, cam1 and cam4 are re-evaluated according to this target, using their own weights compared to each other (not all collections).  It's actually possible a collection can be over target in one round but under the next, triggering a round 3 (cam1 used to do this until a few days ago).  With the final weights determined, files from each collection are deleted until they come in under their collection target.

Right now, the initial round is massively skewed since the this system is new, with cam4 largely above target and cam2 is largely below target.  But over time, this will even out to their respective weights.  The nice part of this system is it's not wasteful if a collection is below target; as I mentioned, cam3 is only used on-demand and will likely always be below its target of 133.57 GiB.  But because of that, it'll always go on to a new round which will utilize the extra space, and the global target will always be 3072 GiB utilization.

The full Python code is available [here](https://gist.github.com/rfinnie/0eccfd7477a6b9cc791f70c0ee621b26).  I'm pretty sure I've got all the corner cases worked out, but obviously please use caution if you use this.  (I've commented out the line used to actually do the delete.)
