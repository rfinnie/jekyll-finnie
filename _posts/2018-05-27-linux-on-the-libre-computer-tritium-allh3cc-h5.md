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

Armbian initially didn't have [an image for the H5](https://www.armbian.com/tritium-h5/), just for the H2+/H3.  The comments on the Kickstarter page were understandably confused, with people trying to use that image on their H5 and getting no video or activity other than a red power LED.  I did some digging and found the ALL-H3-CC H5 is very similar to [Orange Pi PC 2](https://www.armbian.com/orange-pi-pc2/), which had a supported image.  I tried it out, and it mostly worked.  HDMI video (and presumably audio), USB and serial all worked, but Ethernet does not (but a random USB Ethernet adapter I added worked fine).  I posted my findings on the Kickstarter comments section to help people at least verify their hardware is working until official images were produced.

But I didn't want to leave it at that.  What follows is by no means a how-to; more like a stream of observations.  It's also specific to the H5 model, as that's the only one I have.  This post was originally written in 2018, but I came back in 2021 and updated a bunch of it based on my last few years of experience.

---

[U-Boot](https://www.denx.de/wiki/U-Boot) has everything needed for booting, and works beautifully, but you'll need to compile it yourself.  You will need three repositories:

  * [u-boot](https://github.com/u-boot/u-boot)
  * [arm-trusted-firmware](https://github.com/ARM-software/arm-trusted-firmware)
  * [crust](https://github.com/crust-firmware/crust)

Please see instructions and requirements for each; note particularly that crust requires a (as of this writing) custom RISC (not RISC-V) cross-compiler.  Once the requirements are satisfied, here's my compilation process, assuming cross-compiling from an amd64 environment:

``` shell
CRUST_DIR="/home/ryan/git/crust"
ATF_DIR="/home/ryan/git/arm-trusted-firmware"
UBOOT_DIR="/home/ryan/git/u-boot"

make -C "${CRUST_DIR}" clean
make -C "${CRUST_DIR}" libretech_all_h3_cc_h5_defconfig
env PATH="${CRUST_DIR}/or1k-linux-musl/bin:${PATH}" make -C "${CRUST_DIR}" -j16 scp

make -C "${ATF_DIR}" distclean
make -C "${ATF_DIR}" CROSS_COMPILE=aarch64-linux-gnu- PLAT=sun50i_a64 -j16 bl31

make -C "${UBOOT_DIR}" distclean
make -C "${UBOOT_DIR}" libretech_all_h3_cc_h5_defconfig
make -C "${UBOOT_DIR}" CROSS_COMPILE=aarch64-linux-gnu- \
    BL31="${ATF_DIR}/build/sun50i_a64/release/bl31.bin" \
    SCP="${CRUST_DIR}/build/scp/scp.bin" \
    -j16

file "${UBOOT_DIR}/u-boot-sunxi-with-spl.bin"
```

Traditionally, U-Boot works more like [lilo](https://en.wikipedia.org/wiki/LILO_(boot_loader)) did back in the day.  Write a boot script, it points to a kernel and initrd at specific locations and boots them.  If you're fancy, you might have the option to boot a backup kernel.  But U-Boot has a party trick on supported platforms: UEFI booting.  Let's have U-Boot hand off to GRUB to do the actual boot management!

The SoC expects the boot code to start 8 KiB from the beginning of the SD card, but this overlaps the GUID partition table (GPT) by default.  We can get around this by using `gdisk` to set the location of the beginning of the partition table (`fdisk`'s GPT handling doesn't have this functionality). Type `o` to create a new GPT, then `x` to go to the expert menu, then `j` to set the partition table beginning sector.  By default, this is sector 2 (1024 bytes in), but you can change this to sector 6144 (3 MiB in).  This will give plenty of room for U-Boot's loader. Then type `m` to exit the expert mode and continue partitioning as normal, and the created partitions will default to starting at sector 8192 (4 MiB in).

```
Found valid GPT with protective MBR; using GPT.
Disk /dev/mmcblk0: 125042688 sectors, 59.6 GiB
Sector size (logical/physical): 512/512 bytes
Disk identifier (GUID): CEADEC18-8EDC-41D2-973F-2362594061EE
Partition table holds up to 128 entries
Main partition table begins at sector 6144 and ends at sector 6175
First usable sector is 6176, last usable sector is 125042654
Partitions will be aligned on 2048-sector boundaries
Total free space is 2016 sectors (1008.0 KiB)

Number  Start (sector)    End (sector)  Size       Code  Name
   1            8192         1056767   512.0 MiB   EF00  EFI system partition
   2         1056768       125042654   59.1 GiB    8300  Linux filesystem
```

The EFI partition (created with `mkfs.vfat`) layout looks as follows (relative to the partition, which I have mounted as `/boot/efi`):

- /EFI/BOOT/BOOTAA64.EFI
- /EFI/ubuntu/grubaa64.efi

The EFI files are written as part of `grub-install -v --no-nvram /dev/mmcblk0`.  Amazingly, this is all U-Boot needs to hand off to GRUB.  You don't even need a `boot.scr`!  U-Boot's built-in fallback bootscript will notice `BOOTAA64.EFI`, load it along with the DTB (which is built into `u-boot-sunxi-with-spl.bin`) and `bootefi` them.  GRUB then finds its second stage modules and `grub.cfg` in the main partition, and continues on as normal.

To install U-Boot itself, we will manually `dd` it into place at the point in the SD card the SoC expects the boot code to be:

``` shell
dd if=u-boot-sunxi-with-spl.bin of=/dev/mmcblk0 bs=1k seek=8 oflag=sync
```

The implication here is, aside from a manually-compiled U-Boot living in the MBR, I have a completely standard Ubuntu 20.04 LTS ("focal") ARM64 installation running on the Tritium H5.  No extra packages needed, and the kernel is focal's standard 5.4.  `grub-efi-arm64` is installed and managing `grub.cfg`.  USB, SD, Ethernet, serial console, hardware virtualization, graphics, sound and IR have all been tested.

```
michelle
    description: Desktop Computer
    product: Libre Computer Board ALL-H3-CC H5
    serial: 828000018ab9133a
    width: 64 bits
    capabilities: smbios-3.0 dmi-3.0 smp cp15_barrier setend swp tagged_addr_disabled
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
          version: 2021.01
          date: 03/01/2021
          size: 1MiB
          capabilities: pci upgrade bootselect i2oboot
     *-firmware:1
          description: BIOS
          physical id: 303
          size: 1MiB
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
