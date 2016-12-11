---
id: 965
title: 'HOWTO: Encrypt an existing Debian installation'
date: 2009-07-26T02:00:17+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/?p=965
permalink: /2009/07/26/howto-encrypt-an-existing-debian-installation/
openid_comments:
  - 'a:2:{i:0;s:4:"2720";i:1;i:368301;}'
categories:
  - Uncategorized
tags:
  - encryption
  - Finnix
  - linux
---
The last few days have been a snowball of encryption, so to speak. For years, I've done [full disk encryption](http://en.wikipedia.org/wiki/Full_disk_encryption) on my Debian laptop. But this week I decided to look into TrueCrypt's full disk encryption for my Windows desktop. It works surprisingly well ("where's the catch", I've basically been saying to myself), but that started making me follow the data trail.

> I use Acronis True Image to back up to my Debian router / fileserver. OK, True Image has an option to encrypt backups, but what about the other hosts being backed up onto that server? Hell, my laptop is backed up onto there. While the primary purpose of full disk encryption on the laptop is to protect it if it were lost or stolen, it would be nice to have the disk that it's being backed up to be encrypted as well.
> 
> The backup disk is a separate disk on the router, so I could LUKS format it, but that would mean having to manually mount it upon boot. If I'm going to do that, I may as well just encrypt the OS drive of the router, and store the keyfile to the backup drive on there. That way I still have to provide a password upon boot, but it's to boot the entire system.

And with that, I began looking into what it would take to convert a normal Debian system into an encrypted Debian system. Debian-installer provides a way to do full disk encryption during installation, but figuring out what differences were significant took some research.

Note that the instructions below are provided at your own risk. You are copying the system to a temporary location, **destroying your data** and restoring it. A lot could go wrong. I make a lot of assumptions:

  * [Finnix](http://www.finnix.org/) is used throughout the process
  * "Before" system: Single root partition on /dev/sda1, swap on /dev/sda2
  * "After" system: Boot partition on /dev/sda1, encrypted LVM on /dev/sda2, root and swap LVs
  * System name (and Volume Group name) is "nibbler" (Futurama naming scheme here)
  * Data backed up and restored rsync-over-ssh to another system

These assumptions may or may not be close to what you have, so please understand what is happening, rather than blindly paste commands. This procedure was used on a Debian Etch machine, and will work with Lenny (and presumably beyond).

# The procedure

## Prerequisites

Before you reboot into Finnix, be sure your system has the following required packages installed:

<pre># apt-get install lvm2 cryptsetup
</pre>

## Backup

Boot into [Finnix](http://www.finnix.org/), or another LiveCD that contains LVM and cryptsetup w/LUKS support.

Copy the contents of system to another machine.

<pre># mkdir /mnt/drive
# mount /dev/sda1 /mnt/drive
# rsync -az /mnt/drive/ root@backuphost:nibbler-backup/
# umount /mnt/drive
</pre>

**Verify your backup.** The next step will be to **destroy the existing data**, so your backup will be your only copy of your data for a time.

## Drive shredding

Overwrite the drive. Writing it with null bytes is the fastest, but theoretically the least secure, as it's easy to see where encrypted data physically lives on the drive. Using <tt>badblocks</tt>' destructive test mode is slightly better, but it only writes a random pattern, not random data. Still, it has an added benefit of testing your drive at the same time. Writing <tt>/dev/urandom</tt> to the disk is better, but will take a long time. Using GNU <tt>shred</tt> is the safest, but by far the longest. Take your pick of one of these four:

<pre># dd if=/dev/zero of=/dev/sda
# badblocks -v -s -w -t random /dev/sda
# dd if=/dev/urandom of=/dev/sda
# shred /dev/sda
</pre>

## Partitioning, LUKS and LVM

Partition the drive. You will want a <tt>/boot</tt>-sized first partition (128MB or so), and the rest in the second partition. Both should be type 83 (Linux).

<pre># fdisk /dev/sda
...
   Device Boot      Start         End      Blocks   Id  System
/dev/sda1               1          32      257008+  83  Linux
/dev/sda2              33       24321   195101392+  83  Linux
</pre>

Format the first partition.

<pre># mke2fs -j /dev/sda1
</pre>

Encrypt the second partition. The first command will ask for a password to be used to encrypt the partiton. The second command will make the newly created encrypted partition available at <tt>/dev/mapper/sda2_crypt</tt>.

<pre># cryptsetup --verbose --cipher "aes-cbc-essiv:sha256" --key-size 256 --verify-passphrase luksFormat /dev/sda2
# cryptsetup luksOpen /dev/sda2 sda2_crypt
</pre>

Now create the LVM setup using the encrypted partition as the physical volume (PV). In this case, I am using the volume group (VG) name "nibbler", and creating two logical volumes (LVs), "swap_1" (2GB) and "root" (rest of the VG). Replace the "XXXXX" with the number you got from the previous command (the number of Free PEs). The final command activates the LVs, making them available for use.

<pre># pvcreate /dev/mapper/sda2_crypt
# vgcreate nibbler /dev/mapper/sda2_crypt
# lvcreate -L2000 -nswap_1 nibbler
# vgdisplay nibbler | grep "Free  PE"
# lvcreate -lXXXXX -nroot nibbler
# vgchange -a y nibbler
</pre>

Now format the new LVs.

<pre>mkswap /dev/mapper/nibbler-swap_1
mke2fs -j /dev/mapper/nibbler-root
</pre>

## Restore

Mount the root, overlay the (unencrypted) /boot partition, then copy the contents of the system back. Leave the partitions mounted; we've got a lot to do with them.

<pre># mkdir /mnt/dst
# mount /dev/mapper/nibbler-root /mnt/dst
# mkdir /mnt/dst/boot
# mount /dev/sda1 /mnt/dst/boot
# rsync -az root@backuphost:nibbler-backup/ /mnt/dst/
</pre>

## Chroot modifications

Chroot into the root. We'll be doing a lot of things that touch <tt>/proc</tt>, <tt>/sys</tt>, and expect <tt>/dev</tt> to be a certain way, so we'll set those up.

<pre>mount -t proc none /mnt/dst/proc
mount -t sysfs none /mnt/dst/sys
mount --bind /dev /mnt/dst/dev
chroot /mnt/dst
</pre>

Once in the chroot, edit <tt>/etc/crypttab</tt> and add the following line:

<pre>sda2_crypt /dev/sda2 none luks
</pre>

Edit <tt>/etc/fstab</tt> and use the following lines:

<pre>/dev/mapper/nibbler-root /               ext3    errors=remount-ro 0       1
/dev/sda1       /boot           ext2    defaults        0       2
/dev/mapper/nibbler-swap_1 none            swap    sw              0       0
</pre>

Edit <tt>/etc/initramfs-tools/conf.d/resume</tt> and change the "RESUME=..." line to:

<pre>RESUME=/dev/mapper/nibbler-swap_1
</pre>

Edit <tt>/boot/grub/menu.lst</tt> and change all instances of <tt>/dev/sda1</tt> to <tt>/dev/mapper/nibbler-root</tt>.

Now, we'll need to add GRUB to the MBR, and regenerate initrds. Both utilities we'll need rely on <tt>/etc/mtab</tt> being correct, so we'll have to regenerate it. The easiest way is to copy /proc/mounts into it:

<pre>(chroot) # cat /proc/mounts >/etc/mtab
</pre>

And edit it. It should look something like this:

<pre>rootfs / rootfs rw 0 0
none / tmpfs rw,size=801864k 0 0
/ramdisk/dev/hdc /cdrom iso9660 ro 0 0
/dev/loop0 /FINNIX squashfs ro 0 0
none /UNIONFS unionfs rw,dirs=/tmp/UNIONFS=rw:/FINNIX=ro 0 0
none /proc proc rw 0 0
none /sys sysfs rw 0 0
tmpfs /dev tmpfs rw,size=10240k,mode=755 0 0
devpts /dev/pts devpts rw,mode=600 0 0
devshm /dev/shm tmpfs rw 0 0
none /proc/bus/usb usbfs rw 0 0
/dev/mapper/nibbler-root / ext3 rw,errors=continue,data=ordered 0 0
/dev/sda1 /boot ext3 rw,errors=continue,data=ordered 0 0
none /proc proc rw 0 0
none /sys sysfs rw 0 0
tmpfs /dev tmpfs rw,size=10240k,mode=755 0 0
</pre>

Remove all lines before where the "real" stuff begins; that is to say, the partitions you've mounted yourself. You should be left with this:

<pre>/dev/mapper/nibbler-root / ext3 rw,errors=continue,data=ordered 0 0
/dev/sda1 /boot ext3 rw,errors=continue,data=ordered 0 0
none /proc proc rw 0 0
none /sys sysfs rw 0 0
tmpfs /dev tmpfs rw,size=10240k,mode=755 0 0
</pre>

Now you can add GRUB to the MBR:

<pre>(chroot) # grub-install /dev/sda
</pre>

And regenerate the initrds:

<pre>(chroot) # update-initramfs -k all -u
</pre>

Exit the chroot and unmount all used partitions.

<pre>(chroot) # exit
# umount /mnt/dst/proc
# umount /mnt/dst/sys
# umount /mnt/dst/dev
# umount /mnt/dst/boot
# umount /mnt/dst
</pre>

## Reboot & test

Now reboot into your new system! LUKS should ask you for the partition password fairly early, and Debian's initrd and initscripts should seamlessly take care of activating LVM and mounting the correct partitions.