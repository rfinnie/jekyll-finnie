---
author: Ryan Finnie
categories:
- Uncategorized
date: 2015-02-21 17:04:11
guid: http://www.finnie.org/2015/02/21/raspberry-pi-2-ubuntu-raspbian-benchmarks/
id: 2450
layout: post
permalink: /2015/02/21/raspberry-pi-2-ubuntu-raspbian-benchmarks/
tags:
- planet:canonical
title: Raspberry Pi 2 Ubuntu / Raspbian benchmarks
---
One of the nice things about the Raspberry Pi 2 is it has a Cortex-A7-based ARMv7 CPU, as opposed to the original Pi's ARMv6 CPU. This not only allows many more distributions to run on it (as most armhf distributions are compiled to ARMv7 minimum), but also brings with it the performance benefits associated with userland ARMv7 code. [After releasing an Ubuntu 14.04 (trusty) image for the Raspberry Pi 2](http://www.finnie.org/2015/02/16/raspberry-pi-2-update-ubuntu-14-04-image-available/), I decided to pit Raspbian (which uses an ARMv6 userland for compatibility between the original Pi and the Pi 2) against Ubuntu (which is only compiled to ARMv7). I also benchmarked a [Utilite Pro](http://www.compulab.co.il/utilite-computer/web/utilite-models), an ARM system with a faster CPU and built-in SSD, and a modern Intel server.

  * Raspberry Pi B, 700 MHz 1-core BCM2708 CPU, 512 MiB memory, 16 GB SanDisk SDHC Class 4
  * Raspberry Pi 2 B, 900 MHz 4-core BCM2709 CPU, 1 GiB memory, 32 GB SanDisk Ultra Plus microSDHC Class 10 UHS-1
  * Utilite Pro, 1 GHz 4-core i.MX6 CPU, 2 GiB memory, 32 GB SanDisk U110 SSD
  * ASRock Z97 Pro3, 3.5 GHz 4-core Intel Core i5-4690K, 32 GiB memory, 4x 2TB Seagate ST2000DL003 5900 RPM in MD RAID 10

Raspbian wheezy was tested on both Raspberry Pi models, while Ubuntu trusty was also tested on the Raspberry Pi 2, along with the rest of the systems. All installations were current as of today. The systems were tested with [nbench (BYTEmark)](http://www.tux.org/~mayer/linux/bmark.html), OpenSSL and Bonnie++.

## Results

This is a hand-picked assortment of test results; for the full raw results, see below.

<table>
  <tr>
    <th>
      Test
    </th>
    
    <th>
      RPi B<br />Raspbian
    </th>
    
    <th>
      RPi 2<br />Raspbian
    </th>
    
    <th>
      RPi 2<br />Ubuntu
    </th>
    
    <th>
      Utilite<br />Ubuntu
    </th>
    
    <th>
      i5-4690K<br />Ubuntu
    </th>
  </tr>
  
  <tr>
    <th>
      Numeric sort
    </th>
    
    <td>
      217.2
    </td>
    
    <td>
      450.72
    </td>
    
    <td>
      421.55
    </td>
    
    <td>
      334.63
    </td>
    
    <td>
      2,385.1
    </td>
  </tr>
  
  <tr>
    <th>
      FP emulation
    </th>
    
    <td>
      41.334
    </td>
    
    <td>
      70.276
    </td>
    
    <td>
      55.108
    </td>
    
    <td>
      52.454
    </td>
    
    <td>
      795.9
    </td>
  </tr>
  
  <tr>
    <th>
      IDEA
    </th>
    
    <td>
      694.72
    </td>
    
    <td>
      1,308.5
    </td>
    
    <td>
      1,573.3
    </td>
    
    <td>
      1,315
    </td>
    
    <td>
      15,059
    </td>
  </tr>
  
  <tr>
    <th>
      md5 1024
    </th>
    
    <td>
      37,008.46
    </td>
    
    <td>
      62,628.86
    </td>
    
    <td>
      69,563.39
    </td>
    
    <td>
      80,632.53
    </td>
    
    <td>
      670,637.40
    </td>
  </tr>
  
  <tr>
    <th>
      aes-256 cbc 1024
    </th>
    
    <td>
      11,969.50
    </td>
    
    <td>
      18,445.31
    </td>
    
    <td>
      17,295.36
    </td>
    
    <td>
      20,986.47
    </td>
    
    <td>
      124,509.53
    </td>
  </tr>
  
  <tr>
    <th>
      sha512 1024
    </th>
    
    <td>
      8,491.32
    </td>
    
    <td>
      11,838.81
    </td>
    
    <td>
      20,718.25
    </td>
    
    <td>
      25,803.70
    </td>
    
    <td>
      431,647.74
    </td>
  </tr>
  
  <tr>
    <th>
      whirlpool 1024
    </th>
    
    <td>
      1,584.61
    </td>
    
    <td>
      2,949.80
    </td>
    
    <td>
      2,747.05
    </td>
    
    <td>
      2,687.46
    </td>
    
    <td>
      135,009.28
    </td>
  </tr>
  
  <tr>
    <th>
      rsa 1024 verify
    </th>
    
    <td>
      1,540.3
    </td>
    
    <td>
      2,649.6
    </td>
    
    <td>
      2,630.5
    </td>
    
    <td>
      2,890.8
    </td>
    
    <td>
      114,074.5
    </td>
  </tr>
  
  <tr>
    <th>
      ecdsa 256 verify
    </th>
    
    <td>
      73.2
    </td>
    
    <td>
      126.3
    </td>
    
    <td>
      138.0
    </td>
    
    <td>
      161.1
    </td>
    
    <td>
      4,329.6
    </td>
  </tr>
  
  <tr>
    <th>
      Block output
    </th>
    
    <td>
      7,520
    </td>
    
    <td>
      11,028
    </td>
    
    <td>
      11,299
    </td>
    
    <td>
      48,214
    </td>
    
    <td>
      62,762
    </td>
  </tr>
  
  <tr>
    <th>
      Block input
    </th>
    
    <td>
      13,233
    </td>
    
    <td>
      23,015
    </td>
    
    <td>
      22,997
    </td>
    
    <td>
      125,954
    </td>
    
    <td>
      284,914
    </td>
  </tr>
  
  <tr>
    <th>
      Random seeks
    </th>
    
    <td>
      524.7
    </td>
    
    <td>
      1,054
    </td>
    
    <td>
      874.6
    </td>
    
    <td>
      3,218
    </td>
    
    <td>
      444.5
    </td>
  </tr>
</table>

## Notes

  * Interestingly, many of the BYTEmark tests on the Pi 2 were faster on Raspbian than on Ubuntu. But keep in mind that these are tests from the 1990s, and are not taking advantage of modern optimizations (like the floating point emulation test). Many OpenSSL tests performed better on Ubuntu, but not all.
  * Edit: The slower nbench results in Ubuntu appear to be due to a running LSM (Linux Security Module). When Ubuntu is running with AppArmor (default) or SELinux enabled, it's marginally slower than Raspbian, but with LSMs disabled, it's marginally faster than Raspbian. (The Raspbian kernel has no LSM modules compiled in.) I'm keeping these test results as they are because AppArmor is enabled by default, but keep that in mind.
  * Raspbian/Ubuntu aside, virtually all of the tests were faster on the Pi 2 than the original Pi.
  * Bonnie++ tests were roughly the same between Raspbian and Ubuntu on Pi 2, and were decently faster than the original Pi (though in this test an older SDHC card was used for the original Pi, so it's not apples to apples). The SSD on the Utilite blows them away though.
  * All of the CPU tests are single-threaded, and do not take multi-core performance into consideration.
  * This was not a controlled scientific test. I did not run multiple tests on each system and average them together, and in the Intel system's case, it was an active (but low volume) server.
  * All Bonnie++ tests were run with swap disabled and on the boot drive, except for the Intel system where the boot drive (an SSD) did not have enough space for a full test. (Bonnie++ requires twice the amount of RAM as disk to run. On 512 MiB / 1 GiB / 2 GiB systems that's fine, but I didn't have 64 GiB free on the the Intel system's boot drive.

<!--more-->

## Raw results

### RPi B

<pre>BYTEmark* Native Mode Benchmark ver. 2 (10/95)
Index-split by Andrew D. Balsa (11/97)
Linux/Unix* port by Uwe F. Mayer (12/96,11/97)

TEST                : Iterations/sec.  : Old Index   : New Index
                    :                  : Pentium 90* : AMD K6/233*
--------------------:------------------:-------------:------------
NUMERIC SORT        :           217.2  :       5.57  :       1.83
STRING SORT         :          30.638  :      13.69  :       2.12
BITFIELD            :      8.3354e+07  :      14.30  :       2.99
FP EMULATION        :          41.334  :      19.83  :       4.58
FOURIER             :          2359.7  :       2.68  :       1.51
ASSIGNMENT          :          2.5804  :       9.82  :       2.55
IDEA                :          694.72  :      10.63  :       3.15
HUFFMAN             :          428.85  :      11.89  :       3.80
NEURAL NET          :          3.1952  :       5.13  :       2.16
LU DECOMPOSITION    :           75.29  :       3.90  :       2.82
==========================ORIGINAL BYTEMARK RESULTS==========================
INTEGER INDEX       : 11.514
FLOATING-POINT INDEX: 3.773
Baseline (MSDOS*)   : Pentium* 90, 256 KB L2-cache, Watcom* compiler 10.0
==============================LINUX DATA BELOW===============================
CPU                 : ARMv6-compatible processor rev 7 (v6l)
L2 Cache            : 
OS                  : Linux 3.18.7+
C compiler          : gcc version 4.6.3 (Debian 4.6.3-14+rpi1) 
libc                : libc-2.13.so
MEMORY INDEX        : 2.526
INTEGER INDEX       : 3.165
FLOATING-POINT INDEX: 2.093
Baseline (LINUX)    : AMD K6/233*, 512 KB L2-cache, gcc 2.7.2.3, libc-5.4.38
* Trademarks are property of their respective holder.

Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
raspberrypi      1G    42  99  7520  14  4514   8   443  99 13233  11 524.7  51
Latency               276ms    9344ms    5262ms   26123us   16149us   34751us
Version  1.96       ------Sequential Create------ --------Random Create--------
raspberrypi         -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  2586  57 +++++ +++  3886  55  3200  70 +++++ +++  3696  53
Latency              2726us    5095us    4832us    2743us     307us    2468us
1.96,1.96,raspberrypi,1,1424560958,1G,,42,99,7520,14,4514,8,443,99,13233,11,524.7,51,16,,,,,2586,57,+++++,+++,3886,55,3200,70,+++++,+++,3696,53,276ms,9344ms,5262ms,26123us,16149us,34751us,2726us,5095us,4832us,2743us,307us,2468us

OpenSSL 1.0.1e 11 Feb 2013
built on: Fri Jun  6 15:02:47 UTC 2014
options:bn(64,32) rc4(ptr,char) des(idx,cisc,16,long) aes(partial) blowfish(ptr) 
compiler: gcc -fPIC -DOPENSSL_PIC -DZLIB -DOPENSSL_THREADS -D_REENTRANT -DDSO_DLFCN -DHAVE_DLFCN_H -DL_ENDIAN -DTERMIO -g -O2 -fstack-protector --param=ssp-buffer-size=4 -Wformat -Werror=format-security -D_FORTIFY_SOURCE=2 -Wl,-z,relro -Wa,--noexecstack -Wall -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_GF2m -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DAES_ASM -DGHASH_ASM
The 'numbers' are in 1000s of bytes per second processed.
type             16 bytes     64 bytes    256 bytes   1024 bytes   8192 bytes
md2                  0.00         0.00         0.00         0.00         0.00 
mdc2                 0.00         0.00         0.00         0.00         0.00 
md4               2111.92k     7710.99k    23471.96k    48774.36k    70500.68k
md5               1575.49k     5735.79k    17700.13k    37008.46k    53661.71k
hmac(md5)         2724.28k     9222.25k    25068.61k    43978.74k    56001.50k
sha1              1834.41k     6005.60k    15216.97k    24641.76k    30080.26k
rmd160            1391.15k     4389.22k    10595.31k    16489.15k    19804.23k
rc4              34807.13k    40177.89k    41639.59k    42066.47k    42146.33k
des cbc           6241.84k     6592.86k     6662.08k     6715.25k     6682.37k
des ede3          2281.85k     2351.88k     2374.12k     2370.95k     2385.89k
idea cbc             0.00         0.00         0.00         0.00         0.00 
seed cbc          7857.93k     9024.04k     9347.69k     9446.83k     9482.45k
rc2 cbc           6075.61k     6397.57k     6474.49k     6491.95k     6534.29k
rc5-32/12 cbc        0.00         0.00         0.00         0.00         0.00 
blowfish cbc     11636.94k    12964.88k    13267.65k    13459.91k    13439.83k
cast cbc          9786.53k    11425.48k    11879.08k    11967.79k    11967.44k
aes-128 cbc      13080.39k    14961.80k    15568.42k    15649.73k    15723.71k
aes-192 cbc      11587.16k    12963.16k    13444.83k    13519.88k    13551.05k
aes-256 cbc      10360.83k    11460.35k    11866.72k    11969.50k    12059.83k
camellia-128 cbc     8748.67k    10194.79k    10655.76k    10786.59k    10792.07k
camellia-192 cbc     7147.44k     8142.29k     8373.08k     8462.90k     8517.47k
camellia-256 cbc     7123.43k     8075.43k     8361.70k     8479.96k     8507.08k
sha256            3658.39k     8428.37k    14875.23k    18311.67k    19586.83k
sha512            1085.66k     4327.83k     6221.40k     8491.32k     9533.51k
whirlpool          387.65k      786.62k     1299.52k     1584.61k     1685.13k
aes-128 ige      11574.51k    13695.36k    14529.56k    14645.61k    14345.65k
aes-192 ige      10408.42k    12080.82k    12692.24k    12720.55k    12468.83k
aes-256 ige       9362.89k    10807.50k    11218.30k    11325.65k    11100.43k
ghash            15969.05k    17589.81k    18060.24k    18286.30k    18123.77k
                  sign    verify    sign/s verify/s
rsa  512 bits 0.002215s 0.000229s    451.6   4364.5
rsa 1024 bits 0.011254s 0.000649s     88.9   1540.3
rsa 2048 bits 0.073529s 0.002276s     13.6    439.5
rsa 4096 bits 0.535789s 0.008645s      1.9    115.7
                  sign    verify    sign/s verify/s
dsa  512 bits 0.002236s 0.002353s    447.2    424.9
dsa 1024 bits 0.006300s 0.007022s    158.7    142.4
dsa 2048 bits 0.022053s 0.025711s     45.3     38.9
                              sign    verify    sign/s verify/s
 160 bit ecdsa (secp160r1)   0.0018s   0.0059s    569.9    169.5
 192 bit ecdsa (nistp192)   0.0022s   0.0084s    462.0    119.7
 224 bit ecdsa (nistp224)   0.0027s   0.0106s    364.8     94.1
 256 bit ecdsa (nistp256)   0.0032s   0.0137s    307.7     73.2
 384 bit ecdsa (nistp384)   0.0065s   0.0314s    153.8     31.9
 521 bit ecdsa (nistp521)   0.0130s   0.0686s     77.1     14.6
 163 bit ecdsa (nistk163)   0.0047s   0.0170s    212.2     58.7
 233 bit ecdsa (nistk233)   0.0095s   0.0306s    105.4     32.6
 283 bit ecdsa (nistk283)   0.0141s   0.0560s     71.0     17.9
 409 bit ecdsa (nistk409)   0.0350s   0.1238s     28.5      8.1
 571 bit ecdsa (nistk571)   0.0845s   0.2883s     11.8      3.5
 163 bit ecdsa (nistb163)   0.0046s   0.0182s    217.0     55.0
 233 bit ecdsa (nistb233)   0.0093s   0.0333s    107.9     30.0
 283 bit ecdsa (nistb283)   0.0141s   0.0623s     71.0     16.1
 409 bit ecdsa (nistb409)   0.0350s   0.1401s     28.5      7.1
 571 bit ecdsa (nistb571)   0.0844s   0.3290s     11.9      3.0
                              op      op/s
 160 bit ecdh (secp160r1)   0.0050s    200.8
 192 bit ecdh (nistp192)   0.0066s    151.5
 224 bit ecdh (nistp224)   0.0084s    118.8
 256 bit ecdh (nistp256)   0.0110s     91.0
 384 bit ecdh (nistp384)   0.0261s     38.3
 521 bit ecdh (nistp521)   0.0567s     17.6
 163 bit ecdh (nistk163)   0.0082s    121.7
 233 bit ecdh (nistk233)   0.0149s     67.1
 283 bit ecdh (nistk283)   0.0274s     36.4
 409 bit ecdh (nistk409)   0.0611s     16.4
 571 bit ecdh (nistk571)   0.1427s      7.0
 163 bit ecdh (nistb163)   0.0088s    113.1
 233 bit ecdh (nistb233)   0.0164s     60.9
 283 bit ecdh (nistb283)   0.0306s     32.6
 409 bit ecdh (nistb409)   0.0697s     14.4
 571 bit ecdh (nistb571)   0.1634s      6.1
</pre>

### RPi 2 Raspbian

<pre>BYTEmark* Native Mode Benchmark ver. 2 (10/95)
Index-split by Andrew D. Balsa (11/97)
Linux/Unix* port by Uwe F. Mayer (12/96,11/97)

TEST                : Iterations/sec.  : Old Index   : New Index
                    :                  : Pentium 90* : AMD K6/233*
--------------------:------------------:-------------:------------
NUMERIC SORT        :          450.72  :      11.56  :       3.80
STRING SORT         :          36.422  :      16.27  :       2.52
BITFIELD            :      1.2923e+08  :      22.17  :       4.63
FP EMULATION        :          70.276  :      33.72  :       7.78
FOURIER             :          4762.7  :       5.42  :       3.04
ASSIGNMENT          :          6.8898  :      26.22  :       6.80
IDEA                :          1308.5  :      20.01  :       5.94
HUFFMAN             :          657.63  :      18.24  :       5.82
NEURAL NET          :          6.2476  :      10.04  :       4.22
LU DECOMPOSITION    :          229.36  :      11.88  :       8.58
==========================ORIGINAL BYTEMARK RESULTS==========================
INTEGER INDEX       : 20.143
FLOATING-POINT INDEX: 8.644
Baseline (MSDOS*)   : Pentium* 90, 256 KB L2-cache, Watcom* compiler 10.0
==============================LINUX DATA BELOW===============================
CPU                 : 4 CPU ARMv7 Processor rev 5 (v7l)
L2 Cache            : 
OS                  : Linux 3.18.7-v7+
C compiler          : gcc version 4.6.3 (Debian 4.6.3-14+rpi1) 
libc                : libc-2.13.so
MEMORY INDEX        : 4.296
INTEGER INDEX       : 5.654
FLOATING-POINT INDEX: 4.794
Baseline (LINUX)    : AMD K6/233*, 512 KB L2-cache, gcc 2.7.2.3, libc-5.4.38
* Trademarks are property of their respective holder.

Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
raspberrypi      2G    75  98 11028   9  7172   7   507  99 23015   9  1054  47
Latency               613ms    9537ms    6887ms   37024us   29880us   89690us
Version  1.96       ------Sequential Create------ --------Random Create--------
raspberrypi         -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  3861  39 +++++ +++  5097  41  7133  71 +++++ +++  4915  40
Latency               761us    3017us    3273us     694us      82us     619us
1.96,1.96,raspberrypi,1,1424557976,2G,,75,98,11028,9,7172,7,507,99,23015,9,1054,47,16,,,,,3861,39,+++++,+++,5097,41,7133,71,+++++,+++,4915,40,613ms,9537ms,6887ms,37024us,29880us,89690us,761us,3017us,3273us,694us,82us,619us

OpenSSL 1.0.1e 11 Feb 2013
built on: Thu Jan 15 21:10:17 UTC 2015
options:bn(64,32) rc4(ptr,char) des(idx,cisc,16,long) aes(partial) blowfish(ptr) 
compiler: gcc -fPIC -DOPENSSL_PIC -DZLIB -DOPENSSL_THREADS -D_REENTRANT -DDSO_DLFCN -DHAVE_DLFCN_H -DL_ENDIAN -DTERMIO -g -O2 -fstack-protector --param=ssp-buffer-size=4 -Wformat -Werror=format-security -D_FORTIFY_SOURCE=2 -Wl,-z,relro -Wa,--noexecstack -Wall -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_GF2m -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DAES_ASM -DGHASH_ASM
The 'numbers' are in 1000s of bytes per second processed.
type             16 bytes     64 bytes    256 bytes   1024 bytes   8192 bytes
md2                  0.00         0.00         0.00         0.00         0.00 
mdc2                 0.00         0.00         0.00         0.00         0.00 
md4               5729.39k    19130.11k    49388.12k    81406.29k   100783.45k
md5               4438.31k    14612.82k    37866.07k    62628.86k    77651.97k
hmac(md5)         4319.19k    14172.82k    37175.64k    62360.92k    77799.42k
sha1              4468.70k    12776.64k    27218.26k    38258.35k    43382.10k
rmd160            3564.22k     9565.55k    19012.52k    25239.55k    27931.99k
rc4              51118.50k    57082.26k    58688.00k    59210.75k    59361.96k
des cbc          10716.11k    11323.88k    11482.62k    11527.51k    11539.80k
des ede3          4014.53k     4126.68k     4157.87k     4165.97k     4169.73k
idea cbc             0.00         0.00         0.00         0.00         0.00 
seed cbc         12535.65k    13878.08k    14283.09k    14388.91k    14420.65k
rc2 cbc           9081.97k     9540.69k     9663.23k     9694.55k     9704.79k
rc5-32/12 cbc        0.00         0.00         0.00         0.00         0.00 
blowfish cbc     17411.52k    19035.82k    19490.73k    19605.50k    19641.69k
cast cbc         16156.60k    18213.35k    18851.75k    19018.41k    19068.25k
aes-128 cbc      18509.82k    20745.75k    21447.68k    21703.32k    21684.22k
aes-192 cbc      16306.52k    17849.22k    18322.35k    18445.31k    18481.15k
aes-256 cbc      14645.75k    15878.23k    16251.90k    16348.50k    16375.81k
camellia-128 cbc    14842.11k    16692.03k    17303.64k    17463.98k    17511.77k
camellia-192 cbc    12000.46k    13217.69k    13603.50k    13703.17k    13732.52k
camellia-256 cbc    12016.46k    13221.08k    13603.84k    13703.17k    13732.52k
sha256            5168.70k    11715.06k    20019.97k    24473.94k    26167.98k
sha512            1550.01k     6186.62k     8711.34k    11838.81k    13221.89k
whirlpool          758.03k     1546.97k     2503.68k     2949.80k     3116.23k
aes-128 ige      16969.61k    19336.21k    20102.06k    20303.19k    20357.12k
aes-192 ige      14989.08k    16979.78k    17632.68k    17828.18k    17877.67k
aes-256 ige      13574.30k    15186.30k    15722.07k    15861.76k    15900.67k
ghash            24090.86k    26185.30k    27120.21k    27364.69k    27437.74k
                  sign    verify    sign/s verify/s
rsa  512 bits 0.001275s 0.000122s    784.1   8222.0
rsa 1024 bits 0.006798s 0.000377s    147.1   2649.6
rsa 2048 bits 0.045090s 0.001387s     22.2    720.8
rsa 4096 bits 0.332903s 0.005339s      3.0    187.3
                  sign    verify    sign/s verify/s
dsa  512 bits 0.001277s 0.001359s    782.9    735.9
dsa 1024 bits 0.003777s 0.004367s    264.7    229.0
dsa 2048 bits 0.013699s 0.016051s     73.0     62.3
                              sign    verify    sign/s verify/s
 160 bit ecdsa (secp160r1)   0.0009s   0.0034s   1158.6    293.8
 192 bit ecdsa (nistp192)   0.0011s   0.0046s    907.0    217.5
 224 bit ecdsa (nistp224)   0.0014s   0.0061s    724.8    163.6
 256 bit ecdsa (nistp256)   0.0017s   0.0079s    582.0    126.3
 384 bit ecdsa (nistp384)   0.0036s   0.0185s    276.3     53.9
 521 bit ecdsa (nistp521)   0.0074s   0.0408s    135.0     24.5
 163 bit ecdsa (nistk163)   0.0026s   0.0109s    382.4     91.7
 233 bit ecdsa (nistk233)   0.0056s   0.0207s    178.1     48.2
 283 bit ecdsa (nistk283)   0.0087s   0.0382s    114.5     26.2
 409 bit ecdsa (nistk409)   0.0224s   0.0870s     44.6     11.5
 571 bit ecdsa (nistk571)   0.0545s   0.2000s     18.4      5.0
 163 bit ecdsa (nistb163)   0.0026s   0.0116s    384.0     86.1
 233 bit ecdsa (nistb233)   0.0056s   0.0234s    179.7     42.7
 283 bit ecdsa (nistb283)   0.0088s   0.0428s    113.9     23.4
 409 bit ecdsa (nistb409)   0.0225s   0.0999s     44.5     10.0
 571 bit ecdsa (nistb571)   0.0543s   0.2311s     18.4      4.3
                              op      op/s
 160 bit ecdh (secp160r1)   0.0028s    353.3
 192 bit ecdh (nistp192)   0.0039s    258.4
 224 bit ecdh (nistp224)   0.0051s    195.8
 256 bit ecdh (nistp256)   0.0066s    152.4
 384 bit ecdh (nistp384)   0.0155s     64.4
 521 bit ecdh (nistp521)   0.0340s     29.4
 163 bit ecdh (nistk163)   0.0054s    184.8
 233 bit ecdh (nistk233)   0.0105s     95.5
 283 bit ecdh (nistk283)   0.0189s     53.0
 409 bit ecdh (nistk409)   0.0432s     23.2
 571 bit ecdh (nistk571)   0.1002s     10.0
 163 bit ecdh (nistb163)   0.0057s    173.9
 233 bit ecdh (nistb233)   0.0116s     86.5
 283 bit ecdh (nistb283)   0.0211s     47.3
 409 bit ecdh (nistb409)   0.0496s     20.2
 571 bit ecdh (nistb571)   0.1144s      8.7
</pre>

### RPi 2 Ubuntu

<pre>BYTEmark* Native Mode Benchmark ver. 2 (10/95)
Index-split by Andrew D. Balsa (11/97)
Linux/Unix* port by Uwe F. Mayer (12/96,11/97)

TEST                : Iterations/sec.  : Old Index   : New Index
                    :                  : Pentium 90* : AMD K6/233*
--------------------:------------------:-------------:------------
NUMERIC SORT        :          421.55  :      10.81  :       3.55
STRING SORT         :          44.564  :      19.91  :       3.08
BITFIELD            :      1.2551e+08  :      21.53  :       4.50
FP EMULATION        :          55.108  :      26.44  :       6.10
FOURIER             :          3900.8  :       4.44  :       2.49
ASSIGNMENT          :          7.4676  :      28.42  :       7.37
IDEA                :          1573.3  :      24.06  :       7.14
HUFFMAN             :          697.19  :      19.33  :       6.17
NEURAL NET          :          5.6795  :       9.12  :       3.84
LU DECOMPOSITION    :          229.17  :      11.87  :       8.57
==========================ORIGINAL BYTEMARK RESULTS==========================
INTEGER INDEX       : 20.685
FLOATING-POINT INDEX: 7.832
Baseline (MSDOS*)   : Pentium* 90, 256 KB L2-cache, Watcom* compiler 10.0
==============================LINUX DATA BELOW===============================
CPU                 : 4 CPU ARMv7 Processor rev 5 (v7l)
L2 Cache            : 
OS                  : Linux 3.18.0-14-rpi2
C compiler          : gcc version 4.8.2 (Ubuntu/Linaro 4.8.2-19ubuntu1) 
libc                : libc-2.19.so
MEMORY INDEX        : 4.675
INTEGER INDEX       : 5.560
FLOATING-POINT INDEX: 4.344
Baseline (LINUX)    : AMD K6/233*, 512 KB L2-cache, gcc 2.7.2.3, libc-5.4.38
* Trademarks are property of their respective holder.

Version  1.97       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
ubuntu           2G    81  99 11299  12  6663   7   458  99 22997   9 874.6  47
Latency               146ms    1964ms    1969ms   28234us   17446us    4996ms
Version  1.97       ------Sequential Create------ --------Random Create--------
ubuntu              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  5377  53 +++++ +++  9181  73  5538  53 +++++ +++  8974  73
Latency               495us    3662us    3825us     579us     151us     453us
1.97,1.97,ubuntu,1,1424558285,2G,,81,99,11299,12,6663,7,458,99,22997,9,874.6,47,16,,,,,5377,53,+++++,+++,9181,73,5538,53,+++++,+++,8974,73,146ms,1964ms,1969ms,28234us,17446us,4996ms,495us,3662us,3825us,579us,151us,453us

OpenSSL 1.0.1f 6 Jan 2014
built on: Fri Jan  9 18:00:48 UTC 2015
options:bn(64,32) rc4(ptr,char) des(idx,cisc,16,long) aes(partial) blowfish(ptr) 
compiler: cc -fPIC -DOPENSSL_PIC -DOPENSSL_THREADS -D_REENTRANT -DDSO_DLFCN -DHAVE_DLFCN_H -DL_ENDIAN -DTERMIO -g -O2 -fstack-protector --param=ssp-buffer-size=4 -Wformat -Werror=format-security -D_FORTIFY_SOURCE=2 -Wl,-Bsymbolic-functions -Wl,-z,relro -Wa,--noexecstack -Wall -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_GF2m -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DAES_ASM -DGHASH_ASM
The 'numbers' are in 1000s of bytes per second processed.
type             16 bytes     64 bytes    256 bytes   1024 bytes   8192 bytes
md2                  0.00         0.00         0.00         0.00         0.00 
mdc2                 0.00         0.00         0.00         0.00         0.00 
md4               5583.18k    19339.84k    50591.15k    84928.17k   105990.83k
md5               4933.80k    16334.36k    42260.72k    69563.39k    86103.38k
hmac(md5)         5168.09k    16575.19k    42759.08k    70127.27k    86078.81k
sha1              4753.71k    13573.10k    28955.91k    40685.57k    46178.30k
rmd160            4114.97k    11203.16k    22984.95k    30934.70k    34390.07k
rc4              51762.27k    60471.42k    63043.07k    63848.45k    64042.33k
des cbc          11655.27k    12331.43k    12531.33k    12557.65k    12569.26k
des ede3          4439.70k     4544.32k     4571.82k     4578.99k     4573.87k
idea cbc             0.00         0.00         0.00         0.00         0.00 
seed cbc         15028.15k    15992.85k    16226.46k    16152.58k    16134.68k
rc2 cbc           8283.84k     8735.81k     8842.87k     8890.31k     8896.51k
rc5-32/12 cbc        0.00         0.00         0.00         0.00         0.00 
blowfish cbc     17713.68k    19325.95k    19862.27k    19652.61k    19942.06k
cast cbc         17440.01k    18823.55k    19480.49k    19606.87k    19685.38k
aes-128 cbc      20525.47k    22075.99k    22760.96k    22951.94k    23003.14k
aes-192 cbc      17829.68k    19140.84k    19556.78k    19664.21k    19685.38k
aes-256 cbc      15833.37k    16889.54k    17187.84k    17295.36k    17317.89k
camellia-128 cbc    16605.77k    17759.25k    18107.56k    18184.19k    18071.88k
camellia-192 cbc    13084.38k    13919.70k    14156.46k    14218.58k    14196.74k
camellia-256 cbc    13120.85k    13918.42k    14240.68k    14304.60k    14305.96k
sha256            5257.09k    11834.20k    20598.02k    25311.57k    27197.99k
sha512            2573.96k    10302.66k    14994.94k    20718.25k    23317.16k
whirlpool          699.24k     1430.44k     2318.76k     2747.05k     2899.97k
aes-128 ige      19613.12k    21159.32k    21582.68k    21689.69k    21575.92k
aes-192 ige      16952.97k    18264.38k    18650.71k    18749.10k    18746.03k
aes-256 ige      15216.35k    16191.06k    16493.65k    16570.71k    16545.11k
ghash            24319.88k    26480.96k    27397.46k    27801.26k    27863.72k
                  sign    verify    sign/s verify/s
rsa  512 bits 0.001258s 0.000120s    795.1   8336.1
rsa 1024 bits 0.006826s 0.000380s    146.5   2630.5
rsa 2048 bits 0.045500s 0.001403s     22.0    712.9
rsa 4096 bits 0.336000s 0.005411s      3.0    184.8
                  sign    verify    sign/s verify/s
dsa  512 bits 0.001238s 0.001323s    807.7    755.9
dsa 1024 bits 0.003753s 0.004369s    266.5    228.9
dsa 2048 bits 0.013788s 0.016303s     72.5     61.3
                              sign    verify    sign/s verify/s
 160 bit ecdsa (secp160r1)   0.0008s   0.0030s   1315.6    333.1
 192 bit ecdsa (nistp192)   0.0010s   0.0042s   1008.8    240.5
 224 bit ecdsa (nistp224)   0.0013s   0.0056s    793.9    180.0
 256 bit ecdsa (nistp256)   0.0016s   0.0072s    634.6    138.0
 384 bit ecdsa (nistp384)   0.0034s   0.0178s    289.9     56.2
 521 bit ecdsa (nistp521)   0.0072s   0.0400s    138.9     25.0
 163 bit ecdsa (nistk163)   0.0025s   0.0092s    399.7    108.5
 233 bit ecdsa (nistk233)   0.0054s   0.0171s    186.3     58.6
 283 bit ecdsa (nistk283)   0.0084s   0.0313s    119.2     32.0
 409 bit ecdsa (nistk409)   0.0214s   0.0677s     46.7     14.8
 571 bit ecdsa (nistk571)   0.0518s   0.1578s     19.3      6.3
 163 bit ecdsa (nistb163)   0.0025s   0.0100s    402.6     99.8
 233 bit ecdsa (nistb233)   0.0053s   0.0186s    188.4     53.7
 283 bit ecdsa (nistb283)   0.0084s   0.0347s    118.8     28.8
 409 bit ecdsa (nistb409)   0.0215s   0.0764s     46.5     13.1
 571 bit ecdsa (nistb571)   0.0518s   0.1793s     19.3      5.6
                              op      op/s
 160 bit ecdh (secp160r1)   0.0025s    397.3
 192 bit ecdh (nistp192)   0.0035s    287.1
 224 bit ecdh (nistp224)   0.0047s    213.4
 256 bit ecdh (nistp256)   0.0061s    164.8
 384 bit ecdh (nistp384)   0.0148s     67.7
 521 bit ecdh (nistp521)   0.0333s     30.1
 163 bit ecdh (nistk163)   0.0045s    219.8
 233 bit ecdh (nistk233)   0.0084s    119.3
 283 bit ecdh (nistk283)   0.0154s     65.0
 409 bit ecdh (nistk409)   0.0332s     30.1
 571 bit ecdh (nistk571)   0.0778s     12.8
 163 bit ecdh (nistb163)   0.0049s    205.4
 233 bit ecdh (nistb233)   0.0092s    108.9
 283 bit ecdh (nistb283)   0.0171s     58.5
 409 bit ecdh (nistb409)   0.0379s     26.4
 571 bit ecdh (nistb571)   0.0895s     11.2
</pre>

### Utilite

<pre>BYTEmark* Native Mode Benchmark ver. 2 (10/95)
Index-split by Andrew D. Balsa (11/97)
Linux/Unix* port by Uwe F. Mayer (12/96,11/97)

TEST                : Iterations/sec.  : Old Index   : New Index
                    :                  : Pentium 90* : AMD K6/233*
--------------------:------------------:-------------:------------
NUMERIC SORT        :          334.63  :       8.58  :       2.82
STRING SORT         :          51.165  :      22.86  :       3.54
BITFIELD            :      1.2678e+08  :      21.75  :       4.54
FP EMULATION        :          52.454  :      25.17  :       5.81
FOURIER             :          4589.4  :       5.22  :       2.93
ASSIGNMENT          :          5.7939  :      22.05  :       5.72
IDEA                :            1315  :      20.11  :       5.97
HUFFMAN             :          668.33  :      18.53  :       5.92
NEURAL NET          :          6.2869  :      10.10  :       4.25
LU DECOMPOSITION    :          209.07  :      10.83  :       7.82
==========================ORIGINAL BYTEMARK RESULTS==========================
INTEGER INDEX       : 18.965
FLOATING-POINT INDEX: 8.296
Baseline (MSDOS*)   : Pentium* 90, 256 KB L2-cache, Watcom* compiler 10.0
==============================LINUX DATA BELOW===============================
CPU                 : 4 CPU ARMv7 Processor rev 10 (v7l)
L2 Cache            : 
OS                  : Linux 3.13.0-45-generic
C compiler          : gcc version 4.8.2 (Ubuntu/Linaro 4.8.2-19ubuntu1) 
libc                : libc-2.19.so
MEMORY INDEX        : 4.513
INTEGER INDEX       : 4.904
FLOATING-POINT INDEX: 4.601
Baseline (LINUX)    : AMD K6/233*, 512 KB L2-cache, gcc 2.7.2.3, libc-5.4.38
* Trademarks are property of their respective holder.

Version  1.97       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
clamps           4G    96  99 48214  41 38833  57   484  99 125954  97  3218 304
Latency             93438us    2073ms     831ms   38317us    5599us   11133us
Version  1.97       ------Sequential Create------ --------Random Create--------
clamps              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  8359  91 +++++ +++ 12668  89  9318  92 +++++ +++ 11547  92
Latency              7211us    2315us    3450us    2208us      42us    2775us
1.97,1.97,clamps,1,1424586890,4G,,96,99,48214,41,38833,57,484,99,125954,97,3218,304,16,,,,,8359,91,+++++,+++,12668,89,9318,92,+++++,+++,11547,92,93438us,2073ms,831ms,38317us,5599us,11133us,7211us,2315us,3450us,2208us,42us,2775us

OpenSSL 1.0.1f 6 Jan 2014
built on: Fri Jan  9 18:00:48 UTC 2015
options:bn(64,32) rc4(ptr,char) des(idx,cisc,16,long) aes(partial) blowfish(ptr) 
compiler: cc -fPIC -DOPENSSL_PIC -DOPENSSL_THREADS -D_REENTRANT -DDSO_DLFCN -DHAVE_DLFCN_H -DL_ENDIAN -DTERMIO -g -O2 -fstack-protector --param=ssp-buffer-size=4 -Wformat -Werror=format-security
 -D_FORTIFY_SOURCE=2 -Wl,-Bsymbolic-functions -Wl,-z,relro -Wa,--noexecstack -Wall -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_GF2m -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DAES_ASM -DGHASH_ASM
The 'numbers' are in 1000s of bytes per second processed.
type             16 bytes     64 bytes    256 bytes   1024 bytes   8192 bytes
md2                  0.00         0.00         0.00         0.00         0.00 
mdc2                 0.00         0.00         0.00         0.00         0.00 
md4               8241.83k    26442.64k    68072.19k   110937.69k   137616.85k
md5               6614.97k    20734.38k    50776.42k    80632.53k    96689.61k
hmac(md5)         6211.28k    19780.36k    50121.04k    80594.27k    97447.04k
sha1              6224.03k    17443.40k    36604.73k    51314.10k    57698.09k
rmd160            5281.47k    14105.00k    28500.52k    38167.31k    42205.18k
rc4              47155.17k    53723.78k    56523.76k    57421.26k    57431.15k
des cbc          12163.80k    12749.62k    13012.00k    13033.06k    12996.98k
des ede3          4672.83k     4775.56k     4796.24k     4800.45k     4824.83k
idea cbc             0.00         0.00         0.00         0.00         0.00 
seed cbc         15433.24k    16347.68k    16366.75k    16512.74k    16525.96k
rc2 cbc           9267.11k     9746.40k     9817.27k     9917.86k     9891.40k
rc5-32/12 cbc        0.00         0.00         0.00         0.00         0.00 
blowfish cbc     20327.67k    22173.27k    22814.70k    22972.48k    22843.30k
cast cbc         18834.93k    20428.78k    20931.76k    21110.43k    21150.52k
aes-128 cbc      25165.48k    26673.46k    27322.31k    27435.49k    27447.67k
aes-192 cbc      21798.22k    23635.20k    24230.21k    24392.99k    24442.37k
aes-256 cbc      19373.50k    20436.75k    20813.25k    20986.47k    20941.73k
camellia-128 cbc    18807.92k    20145.04k    20569.41k    20652.12k    20675.33k
camellia-192 cbc    15084.09k    15862.17k    16229.40k    16244.03k    16280.12k
camellia-256 cbc    15055.23k    15930.37k    16118.34k    16298.73k    16319.86k
sha256            6813.70k    15169.79k    26092.42k    31481.10k    33762.12k
sha512            3204.16k    12776.06k    18733.28k    25803.70k    28988.92k
whirlpool          692.55k     1408.34k     2275.29k     2687.46k     2845.34k
aes-128 ige      24451.80k    26496.75k    27079.17k    27450.12k    27408.34k
aes-192 ige      21368.84k    22686.98k    23424.82k    23525.57k    23473.80k
aes-256 ige      18886.16k    20038.38k    20458.26k    20566.79k    20646.17k
ghash            32450.51k    35398.26k    36421.72k    36791.07k    36908.85k
                  sign    verify    sign/s verify/s
rsa  512 bits 0.001124s 0.000105s    890.0   9550.8
rsa 1024 bits 0.006208s 0.000346s    161.1   2890.8
rsa 2048 bits 0.041946s 0.001267s     23.8    789.1
rsa 4096 bits 0.302581s 0.004887s      3.3    204.6
                  sign    verify    sign/s verify/s
dsa  512 bits 0.001130s 0.001199s    885.3    833.9
dsa 1024 bits 0.003470s 0.004028s    288.2    248.3
dsa 2048 bits 0.012430s 0.014396s     80.4     69.5
                              sign    verify    sign/s verify/s
 160 bit ecdsa (secp160r1)   0.0007s   0.0026s   1426.5    392.1
 192 bit ecdsa (nistp192)   0.0009s   0.0036s   1103.3    280.3
 224 bit ecdsa (nistp224)   0.0012s   0.0053s    815.7    188.5
 256 bit ecdsa (nistp256)   0.0014s   0.0062s    699.6    161.1
 384 bit ecdsa (nistp384)   0.0033s   0.0163s    304.3     61.4
 521 bit ecdsa (nistp521)   0.0067s   0.0362s    150.0     27.6
 163 bit ecdsa (nistk163)   0.0024s   0.0075s    412.9    132.7
 233 bit ecdsa (nistk233)   0.0050s   0.0137s    201.9     73.2
 283 bit ecdsa (nistk283)   0.0076s   0.0257s    131.1     39.0
 409 bit ecdsa (nistk409)   0.0196s   0.0559s     50.9     17.9
 571 bit ecdsa (nistk571)   0.0511s   0.1296s     19.6      7.7
 163 bit ecdsa (nistb163)   0.0024s   0.0081s    418.2    122.8
 233 bit ecdsa (nistb233)   0.0049s   0.0149s    205.1     67.0
 283 bit ecdsa (nistb283)   0.0076s   0.0286s    131.2     35.0
 409 bit ecdsa (nistb409)   0.0196s   0.0628s     51.1     15.9
 571 bit ecdsa (nistb571)   0.0511s   0.1464s     19.6      6.8
                              op      op/s
 160 bit ecdh (secp160r1)   0.0021s    468.1
 192 bit ecdh (nistp192)   0.0030s    335.2
 224 bit ecdh (nistp224)   0.0044s    226.0
 256 bit ecdh (nistp256)   0.0051s    195.0
 384 bit ecdh (nistp384)   0.0138s     72.7
 521 bit ecdh (nistp521)   0.0301s     33.2
 163 bit ecdh (nistk163)   0.0037s    270.9
 233 bit ecdh (nistk233)   0.0067s    150.1
 283 bit ecdh (nistk283)   0.0127s     78.9
 409 bit ecdh (nistk409)   0.0275s     36.4
 571 bit ecdh (nistk571)   0.0638s     15.7
 163 bit ecdh (nistb163)   0.0040s    250.7
 233 bit ecdh (nistb233)   0.0073s    136.3
 283 bit ecdh (nistb283)   0.0139s     71.9
 409 bit ecdh (nistb409)   0.0311s     32.1
 571 bit ecdh (nistb571)   0.0719s     13.9
</pre>

### i5-4690K

<pre>BYTEmark* Native Mode Benchmark ver. 2 (10/95)
Index-split by Andrew D. Balsa (11/97)
Linux/Unix* port by Uwe F. Mayer (12/96,11/97)

TEST                : Iterations/sec.  : Old Index   : New Index
                    :                  : Pentium 90* : AMD K6/233*
--------------------:------------------:-------------:------------
NUMERIC SORT        :          2385.1  :      61.17  :      20.09
STRING SORT         :          1345.8  :     601.32  :      93.07
BITFIELD            :      7.4081e+08  :     127.07  :      26.54
FP EMULATION        :           795.9  :     381.91  :      88.13
FOURIER             :           58268  :      66.27  :      37.22
ASSIGNMENT          :          78.017  :     296.87  :      77.00
IDEA                :           15059  :     230.33  :      68.39
HUFFMAN             :          6721.5  :     186.39  :      59.52
NEURAL NET          :          136.68  :     219.57  :      92.36
LU DECOMPOSITION    :          3702.2  :     191.79  :     138.49
==========================ORIGINAL BYTEMARK RESULTS==========================
INTEGER INDEX       : 217.125
FLOATING-POINT INDEX: 140.782
Baseline (MSDOS*)   : Pentium* 90, 256 KB L2-cache, Watcom* compiler 10.0
==============================LINUX DATA BELOW===============================
CPU                 : 4 CPU GenuineIntel Intel(R) Core(TM) i5-4690K CPU @ 3.50GHz 3501MHz
L2 Cache            : 6144 KB
OS                  : Linux 3.13.0-45-generic
C compiler          : gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) 
libc                : libc-2.19.so
MEMORY INDEX        : 57.512
INTEGER INDEX       : 51.811
FLOATING-POINT INDEX: 78.084
Baseline (LINUX)    : AMD K6/233*, 512 KB L2-cache, gcc 2.7.2.3, libc-5.4.38
* Trademarks are property of their respective holder.

Version  1.97       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
nibbler      63744M  2717  88 62762   5 96251   3 +++++ +++ 284914   5 444.5   6
Latency             52062us    2482ms   14652ms   36641us   99483us     370ms
Version  1.97       ------Sequential Create------ --------Random Create--------
nibbler             -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  7648   5 +++++ +++  3568   3  2250   5 +++++ +++   628   0
Latency               186ms      81us     386ms     228ms      11us    2314ms
1.97,1.97,nibbler,1,1424568780,63744M,,2717,88,62762,5,96251,3,+++++,+++,284914,5,444.5,6,16,,,,,7648,5,+++++,+++,3568,3,2250,5,+++++,+++,628,0,52062us,2482ms,14652ms,36641us,99483us,370ms,186ms,81us,386ms,228ms,11us,2314ms

OpenSSL 1.0.1f 6 Jan 2014
built on: Fri Jan  9 17:52:48 UTC 2015
options:bn(64,64) rc4(16x,int) des(idx,cisc,16,int) aes(partial) blowfish(idx) 
compiler: cc -fPIC -DOPENSSL_PIC -DOPENSSL_THREADS -D_REENTRANT -DDSO_DLFCN -DHAVE_DLFCN_H -m64 -DL_ENDIAN -DTERMIO -g -O2 -fstack-protector --param=ssp-buffer-size=4 -Wformat -Werror=format-security -D_FORTIFY_SOURCE=2 -Wl,-Bsymbolic-functions -Wl,-z,relro -Wa,--noexecstack -Wall -DMD32_REG_T=int -DOPENSSL_IA32_SSE2 -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_MONT5 -DOPENSSL_BN_ASM_GF2m -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DMD5_ASM -DAES_ASM -DVPAES_ASM -DBSAES_ASM -DWHIRLPOOL_ASM -DGHASH_ASM
The 'numbers' are in 1000s of bytes per second processed.
type             16 bytes     64 bytes    256 bytes   1024 bytes   8192 bytes
md2                  0.00         0.00         0.00         0.00         0.00 
mdc2                 0.00         0.00         0.00         0.00         0.00 
md4             106719.16k   322994.60k   737961.39k  1105796.37k  1289319.77k
md5              74058.79k   213987.90k   470995.74k   670637.40k   767888.04k
hmac(md5)        63472.70k   190996.20k   442288.12k   656018.77k   766369.79k
sha1             86987.60k   239977.86k   509936.55k   741900.97k   902851.55k
rmd160           50582.59k   120868.69k   218979.15k   273907.03k   295291.56k
rc4             439214.15k   793583.10k   931643.08k   959178.75k   965929.64k
des cbc          82232.33k    85451.92k    85785.86k    85917.01k    86350.26k
des ede3         31638.78k    32192.49k    32414.31k    32370.01k    32385.71k
idea cbc             0.00         0.00         0.00         0.00         0.00 
seed cbc         85378.73k    86113.46k    85957.63k    85827.58k    86218.75k
rc2 cbc          52713.13k    53636.61k    53993.22k    54268.92k    54160.04k
rc5-32/12 cbc        0.00         0.00         0.00         0.00         0.00 
blowfish cbc    132890.61k   140962.25k   142400.85k   143064.75k   143606.58k
cast cbc        123477.80k   131612.07k   133600.00k   134423.46k   134250.50k
aes-128 cbc     155945.39k   170214.83k   173895.42k   175899.91k   175699.29k
aes-192 cbc     131270.06k   142201.22k   144801.02k   146176.86k   146210.82k
aes-256 cbc     113957.72k   122551.72k   124043.69k   124509.53k   124903.42k
camellia-128 cbc   116319.99k   179128.77k   204550.83k   211808.60k   214896.16k
camellia-192 cbc   100422.48k   140049.42k   154080.68k   158968.15k   161086.50k
camellia-256 cbc    97688.27k   138615.62k   153032.19k   159233.03k   160516.78k
sha256           61247.67k   136223.06k   235369.64k   291570.01k   307383.57k
sha512           49566.23k   198618.43k   307382.45k   431647.74k   475592.02k
whirlpool        32330.11k    68737.35k   113109.50k   135009.28k   143833.99k
aes-128 ige     157819.94k   164321.30k   165555.97k   166157.53k   166488.75k
aes-192 ige     134085.14k   138452.57k   139268.27k   139497.57k   139569.83k
aes-256 ige     115722.84k   119265.58k   120394.66k   119833.26k   120444.25k
ghash          1555713.02k  3315741.80k  3619220.55k  3641529.34k  3646270.12k
                  sign    verify    sign/s verify/s
rsa  512 bits 0.000038s 0.000003s  26399.4 303787.7
rsa 1024 bits 0.000126s 0.000009s   7913.9 114074.5
rsa 2048 bits 0.000956s 0.000029s   1046.3  34247.7
rsa 4096 bits 0.006810s 0.000108s    146.8   9296.6
                  sign    verify    sign/s verify/s
dsa  512 bits 0.000040s 0.000037s  25145.5  27289.8
dsa 1024 bits 0.000092s 0.000101s  10874.2   9864.7
dsa 2048 bits 0.000286s 0.000335s   3491.0   2984.4
                              sign    verify    sign/s verify/s
 160 bit ecdsa (secp160r1)   0.0000s   0.0002s  20603.9   5858.8
 192 bit ecdsa (nistp192)   0.0001s   0.0002s  17141.2   4853.3
 224 bit ecdsa (nistp224)   0.0001s   0.0001s  16441.4   7415.1
 256 bit ecdsa (nistp256)   0.0001s   0.0002s  10583.9   4329.6
 384 bit ecdsa (nistp384)   0.0002s   0.0007s   6151.7   1471.3
 521 bit ecdsa (nistp521)   0.0004s   0.0009s   2664.9   1151.5
 163 bit ecdsa (nistk163)   0.0001s   0.0003s   7021.2   2944.9
 233 bit ecdsa (nistk233)   0.0003s   0.0004s   3571.8   2255.8
 283 bit ecdsa (nistk283)   0.0004s   0.0008s   2352.1   1230.7
 409 bit ecdsa (nistk409)   0.0010s   0.0013s   1029.0    759.2
 571 bit ecdsa (nistk571)   0.0021s   0.0031s    477.4    318.9
 163 bit ecdsa (nistb163)   0.0001s   0.0004s   7056.1   2785.4
 233 bit ecdsa (nistb233)   0.0003s   0.0005s   3609.1   2114.1
 283 bit ecdsa (nistb283)   0.0004s   0.0009s   2340.8   1156.8
 409 bit ecdsa (nistb409)   0.0010s   0.0014s   1024.7    716.2
 571 bit ecdsa (nistb571)   0.0021s   0.0034s    477.6    294.6
                              op      op/s
 160 bit ecdh (secp160r1)   0.0001s   7288.2
 192 bit ecdh (nistp192)   0.0002s   6045.6
 224 bit ecdh (nistp224)   0.0001s  10659.0
 256 bit ecdh (nistp256)   0.0002s   5927.2
 384 bit ecdh (nistp384)   0.0006s   1779.3
 521 bit ecdh (nistp521)   0.0006s   1610.1
 163 bit ecdh (nistk163)   0.0002s   6214.0
 233 bit ecdh (nistk233)   0.0002s   4779.5
 283 bit ecdh (nistk283)   0.0004s   2560.5
 409 bit ecdh (nistk409)   0.0006s   1597.7
 571 bit ecdh (nistk571)   0.0015s    657.1
 163 bit ecdh (nistb163)   0.0002s   5777.9
 233 bit ecdh (nistb233)   0.0002s   4529.4
 283 bit ecdh (nistb283)   0.0004s   2395.6
 409 bit ecdh (nistb409)   0.0007s   1479.1
 571 bit ecdh (nistb571)   0.0017s    599.0
</pre>
