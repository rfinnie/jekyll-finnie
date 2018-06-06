---
date: 2018-05-27 16:23:33-07:00
excerpt: Wherein Ryan has many more ARMs than arms.
excerpt_standalone: true
headline_image: /blog-media/2018/all-h3-cc-h5.jpg
layout: post
title: Linux on the Libre Computer Tritium ALL-H3-CC H5
---
Linux on ARM hardware has been a bit of a hobby of mine for years now.  My first ARM Linux installation was on a Linksys NSLU2 NAS.  It was horribly, unbelievably slow.  But hey, it was fun.

I've got a [Cubietruck](https://linux-sunxi.org/Cubietech_Cubietruck) in my closet.  Two [Utilitie](https://en.wikipedia.org/wiki/Utilite)s.  More Raspberry Pis than I know what to do with.  We've got a fair number of ARM systems at work.  Up until about 5 years ago they were mostly assorted vendor devel / eval boards, and we maintained an internal wiki page for dealing with them, named WhichArmsSuckTheMost.  Most of those were killed in favor of [Calxeda](https://en.wikipedia.org/wiki/Calxeda) blades, which looked to be the future at the time.  Sadly, Calxeda went out of business, but we still have a few clusters left.  These days the new hotness is [HP Moonshot](https://www.hpe.com/us/en/servers/moonshot.html), and the specs on those are humbling even by normal server standards.  (8 cores, 64 GiB RAM, and a modest 250GB SSD per compute cartridge.  Multiply by **45** for every 2U chassis.)

Last year I found a Kickstarter for the [Libre Computer Tritium](https://www.kickstarter.com/projects/librecomputer/libre-computer-board-tritium-sbc-linux-android-7-n), a series of Allwinner-based ARM boards in a Raspberry Pi 3-compatible form factor.  I backed the 2 GiB RAM model, which was expected to be fulfilled in January.  That target was missed since, well, it's a Kickstarter project, and I eventually forgot about it, until it unexpectedly showed up in my mailbox Friday.

<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2018/all-h3-cc-h5.jpg" alt="Libre Computer Tritium ALL-H3-CC H5" class="img-responsive img-rounded img-lg">

The hardware is slick and good quality, and indeed it fits in standard Raspberry Pi cases (though the chip layout isn't identical, so cases which expect e.g. a heatsinked SoC chip to be in a certain place won't be compatible).  When it arrived, I started looking at what to do with it.

Here's where the company sort of dropped the ball, in my opinion.  They offer 3 models: a 512MiB Allwinner H2+ and a 1 GiB Allwinner H3 (the H2+ and H3 are effectively identical 32-bit SoCs), and a 2 GiB 64-bit Allwinner H5 (which is what I got).  The H5 is effectively a completely different and incompatible SoC to the H2+/H3.  Further confusing the matter is the model name for all three is "ALL-H3-CC".

As of this writing, there is only a single [Armbian image](https://www.armbian.com/tritium-h3/), but just for the H2+/H3.  The comments on the Kickstarter page are understandably confused, with people trying to use that image on their H5 and getting no video or activity other than a red power LED.  I did some digging and found the ALL-H3-CC H5 is very similar to [Orange Pi PC 2](https://www.armbian.com/orange-pi-pc2/), which has a supported image.  I tried it out, and it mostly works.  HDMI video (and presumably audio), USB and serial all work, but Ethernet does not (but a random USB Ethernet adapter I added worked fine).  I posted my findings on the Kickstarter comments section to help people at least verify their hardware is working until official images are produced.

**Update**: As of 2018-06-06, Armbian does not yet have a Tritium H5 image, but LoveRPi has produced [Tritium H3/H5 images based on Armbian](http://share.loverpi.com/board/libre-computer-project/libre-computer-board-all-h3-cc/image/ubuntu/).

But I didn't want to leave it at that.  What follows is by no means a how-to; more like a stream of observations.  It's also specific to the H5 model, as that's the only one I have.

Turns out [U-Boot](https://www.denx.de/wiki/U-Boot) has everything needed for booting, and works beautifully.  It was added just after v2018.05, so you'll need to compile from [git HEAD](https://www.denx.de/wiki/U-Boot/SourceCode), using `libretech_all_h3_cc_h5_defconfig`.  [This page](https://github.com/apritzel/pine64/) goes into detail about the whats and whys of booting on Allwinner A53-based SoCs (it's specific to the Pine64 platform, but the concepts all translate to the H5).

Traditionally, U-Boot works more like [lilo](https://en.wikipedia.org/wiki/LILO_(boot_loader)) did back in the day.  Write a boot script, it points to a kernel and initrd at specific locations and boots them.  If you're fancy, you might have the option to boot a backup kernel.  But U-Boot has a recent party trick on supported platforms: UEFI booting.  Let's have U-Boot hand off to GRUB to do the actual boot management!

On my system, the first partition on the SD card is 2048 512-byte sectors in, 512 MiB long, MBR partition type "ef", vfat-formatted.  2048 for the first sector has been the default for a long time, but is important here since the SoC expects the boot code to be 8 KiB from the beginning of the disk, and that's where you wrote it to as part of the U-Boot compilation above.  MBR is recommended since, by default, 8 KiB in on a GPT is actively used for partition data.  (I didn't even know an EFI System Partition was even possible on an MBR until today; thought it was GPT only.)  512 MiB length is quite overkill, but I'd recommend at least 64 MiB.

**Update**: If you wish to use GPT, it *is* possible to make it so the area between 8 KiB and 1 MiB is not used by the GPT.  This functionality does not appear to be in stock `fdisk`'s GPT handling, but you can do it in `gdisk`.  Type `o` to create a new GPT, then `x` to go to the expert menu, then `j` to set the partition table beginning sector.  By default, this is sector 2 (1024 bytes in), but you can change this to sector 2048 (1 MiB in).  Then type `m` to exit the expert mode and continue partitioning as normal, and the created partitions will default to starting at sector 4096.

The EFI partition layout looks as follows (relative to the partition, which I have mounted as `/boot/efi`):

```
/EFI/BOOT/BOOTAA64.EFI
/EFI/ubuntu/grubaa64.efi
/dtb/allwinner/sun50i-h5-libretech-all-h3-cc.dtb
```

The EFI files are written as part of `grub-install -v --no-nvram /dev/mmcblk0`, and the DTB comes from the U-Boot compilation.  Amazingly, this is all U-Boot needs to hand off to GRUB.  You don't even need a `boot.scr`!  U-Boot's built-in fallback bootscript will notice `BOOTAA64.EFI`, load it along with the DTB and `bootefi` them.  GRUB then finds its second stage modules and `grub.cfg` in the main partition, and continues on as normal.

The implication here is, aside from a HEAD-compiled U-Boot living in the MBR, I have a completely standard Ubuntu 18.04 LTS ("bionic") ARM64 installation running on the Tritium H5.  No extra packages needed, and the kernel is bionic's standard 4.15.  `grub-efi-arm64` is installed and managing `grub.cfg`.  USB, SD, Ethernet, serial console and hardware virtualization have all been tested.  IR hasn't been tested (but I see it in dmesg).  Sound hasn't been tested, and video turns off as soon as kernel init is done, but that's OK for me as I'm using it in a server capacity.  The only major problem is the kernel will panic right before shutdown/reboot, so a hands-off remote reboot is not possible.

[Full `lshw` output is here](https://pastebin.ubuntu.com/p/YBRGJPzYG7/), but in short:

```
michelle
    description: Desktop Computer
    product: Libre Computer Board ALL-H3-CC H5
    vendor: sunxi
    serial: 828000018ab9133a
    width: 64 bits
    capabilities: smbios-3.0 dmi-3.0 smp cp15_barrier setend swp
    configuration: chassis=desktop uuid=38323830-3030-3031-3861-623931333361
  *-core
       description: Motherboard
       product: sunxi
       vendor: sunxi
       physical id: 0
     *-firmware:0
          description: BIOS
          vendor: U-Boot
          physical id: 0
          version: 2018.05-00424-g2a8e80dfce
          date: 05/27/2018
          size: 1MiB
          capabilities: pci upgrade bootselect i2oboot
     *-firmware:1
          description: BIOS
          physical id: 1
          size: 1MiB
          capabilities: pcmcia shadowing escd cdboot socketedrom
     *-cpu:{0,1,2,3}
          description: CPU
          product: cpu
          physical id: {2,3,4,5}
          bus info: cpu@{0,1,2,3}
          capabilities: fp asimd evtstrm aes pmull sha1 sha2 crc32 cpuid
     *-memory
          description: System memory
          physical id: 6
          size: 1995MiB
```

(Futurama naming scheme, if you're wondering about the hostname.)

If you're one of the approximately 500 people who got an H5 as part of the Kickstarter, I hope you find this information useful.