---
id: 2420
title: Ubuntu 14.04 (Trusty Tahr) on the Raspberry Pi 2
date: 2015-02-14T00:32:51+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/2015/02/14/ubuntu-14-04-trusty-tahr-on-the-raspberry-pi-2/
permalink: /2015/02/14/ubuntu-14-04-trusty-tahr-on-the-raspberry-pi-2/
categories:
  - Uncategorized
tags:
  - planet:canonical
---
UPDATE: [I've created a proper Ubuntu 14.04 image for the Raspberry Pi 2.](http://www.finnie.org/2015/02/16/raspberry-pi-2-update-ubuntu-14-04-image-available/)

My [Raspberry Pi 2](http://www.raspberrypi.org/raspberry-pi-2-on-sale/) arrived yesterday, and I started playing with it today. Unlike the original Raspberry Pi which had an ARMv6 CPU, the Raspberry Pi 2 uses a Broadcom BCM2836 (ARMv7) CPU, which allows for binary compatibility with many distributions' armhf ports. However, it's still early early in the game, and since ARM systems have little standardization, there isn't much available yet. [Raspbian](http://raspbian.org/) works, but its userland still uses ARMv6-optimized binaries. Ubuntu has an early beta of [Ubuntu Snappy](http://www.ubuntu.com/cloud/tools/snappy), but Snappy is a much different environment than "regular" Ubuntu.

I found [this post by Sjoerd Simons](http://sjoerd.luon.net/posts/2015/02/debian-jessie-on-rpi2/) detailing getting Debian testing (jessie) on the Pi 2, and he did a good job of putting together the needed software, which I used to get a clean working install of Ubuntu trusty on my Pi 2. This is meant as a rough guide, mostly from memory -- I'll let better people eventually take care of producing a user-friendly system image. This procedure should work for trusty, utopic, and vivid, and might work for earlier distributions.
  
<!--more-->


  
1) Install and boot Raspbian on one MicroSD card, and get the system access to a second MicroSD card, e.g. via a USB adapter.

2) Partition and format the card. #1 must be a vfat partition (64MB should be fine), and for the rest of this post I'm going to assume #2 is a swap partition and #3 is the rest of the card as ext4 for the root partition. Mount the root partition at e.g. /mnt/ubuntu and the vfat partition at e.g. /mnt/ubuntu-boot

3) Bootstrap a trusty system.

<pre># apt-get install debootstrap
# ln -sf gutsy /usr/share/debootstrap/scripts/trusty
# debootstrap trusty /mnt/ubuntu
</pre>

4) Chroot into the bootstrapped system.

<pre># mount -t proc none /mnt/ubuntu/proc
# mount -t sysfs none /mnt/ubuntu/sys
# mount --bind /dev /mnt/ubuntu/dev
# chroot /mnt/ubuntu
</pre>

5) Configure the base system. At the very least you should add a user, set passwords, and edit /etc/fstab as so:

<pre>proc            /proc           proc    defaults          0       0
/dev/mmcblk0p3  /               ext4    defaults,noatime  0       1
/dev/mmcblk0p2  none            swap    sw                0       0
</pre>

6) Install all the necessary packages in [Sjoerd's repository](https://repositories.collabora.co.uk/debian/). You can either add the apt repository, or just do as I did and download/dpkg install them manually. However, there's a problem. These are built for jessie, and the linux-image package requires initramfs-tools 0.110 or greater, which is not yet available in Ubuntu. But since the kernel we're using doesn't actually need an initramfs, I've put together an ["initramfs-tools 0.110~fake1"](http://www.finnie.org/software/raspberrypi/initramfs-tools_0.110~fake1_all.deb) package which fakes it. Simply install that package ahead of time.

7) Outside the chroot, copy /boot/config.txt from the Raspbian installation to /mnt/ubuntu/boot/firmware/config.txt. Then create /mnt/ubuntu/boot/firmware/cmdline.txt with the following:

<pre>dwc_otg.lpm_enable=0 console=ttyAMA0,115200 console=tty1 root=/dev/mmcblk0p3 rootwait
</pre>

8) Copy the firmware files to the actual boot device:

<pre># cp -a /mnt/ubuntu/boot/firmware/* /mnt/ubuntu-boot/
</pre>

9) Umount everything, shut down the system, swap the new card into place, and boot. Once you are successfully booted into the Ubuntu system, I'd recommend installing the ubuntu-standard metapackage to get everything not pulled in by the base debootstrap.

<pre>ryan@raspberrypi:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 14.04.1 LTS
Release:        14.04
Codename:       trusty

ryan@raspberrypi:~$ uname -a
Linux raspberrypi 3.18.0-trunk-rpi2 #1 SMP PREEMPT Debian 3.18.5-1~exp1.co1 (2015-02-02) armv7l armv7l armv7l GNU/Linux

ryan@raspberrypi:~$ cat /proc/cpuinfo 
processor       : [0,1,2,3]
model name      : ARMv7 Processor rev 5 (v7l)
BogoMIPS        : 38.40
Features        : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm 
CPU implementer : 0x41
CPU architecture: 7
CPU variant     : 0x0
CPU part        : 0xc07
CPU revision    : 5

Hardware        : BCM2709
Revision        : 0000
Serial          : 0000000000000000

ryan@raspberrypi:~$ gcc -v
Using built-in specs.
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/lib/gcc/arm-linux-gnueabihf/4.8/lto-wrapper
Target: arm-linux-gnueabihf
Configured with: ../src/configure -v --with-pkgversion='Ubuntu/Linaro 4.8.2-19ubuntu1' --with-bugurl=file:///usr/share/doc/gcc-4.8/README.Bugs --enable-languages=c,c++,java,go,d,fortran,objc,obj-c++ --prefix=/usr --program-suffix=-4.8 --enable-shared --enable-linker-build-id --libexecdir=/usr/lib --without-included-gettext --enable-threads=posix --with-gxx-include-dir=/usr/include/c++/4.8 --libdir=/usr/lib --enable-nls --with-sysroot=/ --enable-clocale=gnu --enable-libstdcxx-debug --enable-libstdcxx-time=yes --enable-gnu-unique-object --disable-libmudflap --disable-libitm --disable-libquadmath --enable-plugin --with-system-zlib --disable-browser-plugin --enable-java-awt=gtk --enable-gtk-cairo --with-java-home=/usr/lib/jvm/java-1.5.0-gcj-4.8-armhf/jre --enable-java-home --with-jvm-root-dir=/usr/lib/jvm/java-1.5.0-gcj-4.8-armhf --with-jvm-jar-dir=/usr/lib/jvm-exports/java-1.5.0-gcj-4.8-armhf --with-arch-directory=arm --with-ecj-jar=/usr/share/java/eclipse-ecj.jar --enable-objc-gc --enable-multiarch --enable-multilib --disable-sjlj-exceptions --with-arch=armv7-a --with-fpu=vfpv3-d16 --with-float=hard --with-mode=thumb --disable-werror --enable-checking=release --build=arm-linux-gnueabihf --host=arm-linux-gnueabihf --target=arm-linux-gnueabihf
Thread model: posix
gcc version 4.8.2 (Ubuntu/Linaro 4.8.2-19ubuntu1)
</pre>
