---
date: 2009-07-26 15:14:09-07:00
layout: post
title: Keyfile-based LUKS encryption in Debian
wp_id: 982
---
***Edit (2022-05-01):** While this post is 13 years old as of this edit, I still use it as a reference for creating new arrays, mostly by searching "site:finnie.org luks".  I've updated the example cipher from aes-cbc-essiv:sha256 (256 bit key size) to aes-xts-plain64 (512 bit key size) to reflect modern best practices, but keep in mind there is probably other parts of this post which are out of date.*

As a follow-up to [yesterday's post]({{ site.url }}{{ site.baseurl }}{% post_url 2009-07-26-howto-encrypt-an-existing-debian-installation %}), here is the "Debian way" to do multiple LUKS-encrypted partitions on a system. In my case, I wanted to add a second drive, a backup storage drive, to my router. LUKS allows for multiple passwords or keyfiles to unlock a partition, which can be used to automate mounting. First, create a LUKS partition as normal (<tt>/dev/sdb1</tt> assumed here):

<pre># cryptsetup --verbose --type luks2 --pbkdf argon2id --cipher aes-xts-plain64 --key-size 512 --verify-passphrase luksFormat /dev/sdb1
</pre>

We'll need the key size, which I'm specifying in this case (512 bits), but if you're using an existing LUKS partition, run this command and look for "MK bits":

<pre># cryptsetup luksDump /dev/sdb1
</pre>

512 bits is 64 bytes, so we will need a 64-byte keyfile.

<pre># dd if=/dev/random of=/etc/keys/sdb1_key bs=1 count=64
</pre>

(Update, 2012-11-02: Previously that above line said "bs=64 count=1". However, it appears if the RNG does not have enough entropy, dd will not wait around, and will instead just output the bytes it has. "bs=1 count=64" will force dd to block until it has all 64 bytes.)

Now add the keyfile to the partition. This can be done while the partition is loaded, mounted and in use. This command will ask you for one of your existing passphrases on the partition to gain access. Once you supply the phassphrase, the keyfile will be added to an open slot.

<pre># cryptsetup luksAddKey /dev/sdb1 /etc/keys/sdb1_key
</pre>

From this point, you can now unlock the partition using your existing passphrase, or you can specify the keyfile:

<pre># cryptsetup --key-file /etc/keys/sdb1_key luksOpen /dev/sdb1 sdb1_crypt
</pre>

Now for the Debian-specific part. If you would like this partition to be used upon bootup, add a line to <tt>/etc/crypttab</tt>:

<pre>sdb1_crypt /dev/sdb1 /etc/keys/sdb1_key luks
</pre>

And then add a line to <tt>/etc/fstab</tt>:

<pre>/dev/mapper/sdb1_crypt  /opt/bigdrive   ext3    defaults                0       0
</pre>

Debian (through <tt>/etc/init.d/cryptdisks</tt>) will take care of the luksOpen for you, and at that point <tt>/dev/mapper/sdb1_crypt</tt> is mounted as usual.

**Safeguard this keyfile!** There is no difference between the keyfile and a passphrase. Either will allow full access to the encrypted partition.

You could actually create a LUKS partition that is _only_ keyfile-based, but you are out of luck if you lose the keyfile. In my case, I'm encrypting the backup drive, and storing the key on the backup server. If the backup server itself were to fail, I'd need a passphrase fallback to mount the backup drive to recover the backup server.
