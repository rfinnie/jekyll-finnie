---
id: 2363
title: Compiling BusyBox with uClibc
date: 2014-02-13T22:14:27+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/2014/02/13/compiling-busybox-with-uclibc/
permalink: /2014/02/13/compiling-busybox-with-uclibc/
categories:
  - Uncategorized
---
Finnix uses [BusyBox](http://www.busybox.net/) for its initrd, and that BusyBox installation [requires a custom patch](http://bazaar.launchpad.net/~finnix/finnix/neale-initrd-pkg/view/head:/patches/busybox/busybox-1.19.4-sysmodules.patch). In the past I've compiled BusyBox with [uClibc](http://uclibc.org/) to keep the size down: a full static BusyBox binary is about 700 KB when compiled against uClibc, and about 1.8 MB when compiled against glibc. For a LiveCD distribution which prides itself on its balance between size and features, 1.1 MB of unneeded bloat is a lot.

In the past, I've used a development [Buildroot](http://buildroot.uclibc.org/) chroot to compile BusyBox, as the documentation recommends. However, the pre-built chroots on the Buildroot site are quite ancient, and I never had success in having buildroot compile an equivalently-featured chroot. On top of that, recently they deprecated building a target toolchain in the chroot itself.

See, it's not just a matter of "build against uClibc". The libc is tightly coupled to the toolchain, a set of utilities such as gcc, binutils, etc. In my case, I don't care about cross-compilation and just wanted to build a native uClibc development toolchain chroot on each supported Finnix architecture (x86, amd64 and powerpc) for the purpose of building BusyBox.

As it turns out, I don't need the full chroot, just the toolchain, and it's rather easy to do, though a little time consuming and not immediately obvious.

Download and extract Buildroot and BusyBox. Here we're assuming:

<pre>BUILDROOTDIR=/home/ryan/buildroot/buildroot-2013.11
BUSYBOXDIR=/home/ryan/buildroot/busybox-1.20.2
</pre>

Configure Buildroot:

<pre>cd $BUILDROOTDIR
make menuconfig
</pre>

Be sure to select the proper architecture information under "Target options". Since Buildroot is primarily a cross-compilation tool, it defaults to i386, not the host target. Indeed, you can even use this method to cross-compile BusyBox.

Under "Toolchain", enable the required uClibc options. In my case, my rather fully-featured BusyBox config required the following toolchain options:

<pre>[*] Enable large file (files > 2 GB) support
  [*] Enable IPv6 support
  [*] Enable RPC support
</pre>

That should be it! You don't need to bother with anything under "Target packages", since we're not actually going to be compiling the full target chroot, just the toolchain. Start the build:

<pre>make clean && time make toolchain
</pre>

This will download a bunch of sources and will require some time to compile; on my relatively fast AMD64 build environment, the build portion takes about 10 minutes.

Once done, the uClibc toolchain is in <tt>output/host/</tt>. You may use this to compile BusyBox:

<pre>cd $BUSYBOXDIR
make menuconfig
export PATH=$BUILDROOTDIR/output/host/usr/bin:$PATH
make clean && time make CROSS_COMPILE=$(cd $BUILDROOTDIR/output/host/usr && ls -d -1 *-buildroot-linux-uclibc)- busybox
</pre>

Note that the toolchain is [not easily relocatable](http://buildroot.uclibc.org/downloads/manual/manual.chunked/ch05.html#_using_the_generated_toolchain_outside_buildroot).
