---
date: 2020-09-29 14:03:43-07:00
excerpt: If you want to anonymize IP addresses, for example in HTTP logs, here's the
  method I recommend.
excerpt_standalone: false
layout: post
title: The "perfect" IP hashing algorithm
---
If you want to anonymize IP addresses, for example in HTTP logs, here's the method I recommend:

``` python
#!/usr/bin/env python3

import hashlib
import ipaddress
import sys


KEY = b"example"


def hash_ip(ip, key=b""):
    last_len = -8 if ip.version == 6 else -1
    base_bytes = ip.packed[:last_len]
    last_bytes = ip.packed[last_len:]
    base_bhash = hashlib.shake_256(key + base_bytes).digest(len(base_bytes))
    last_bhash = hashlib.shake_256(key + ip.packed).digest(len(last_bytes))
    return ipaddress.ip_address(base_bhash + last_bhash)


for line in sys.stdin:
    line_ip, line_rest = line.split(" ", 1)
    ip_hashed = hash_ip(ipaddress.ip_address(line_ip), KEY)
    sys.stdout.write("{} {}".format(ip_hashed, line_rest))
```

Notes:

* IPv4 and IPv6 addresses are both supported.
* Hashed IPv4 addresses return IPv4, and hashed IPv6 addresses return IPv6.  However, that doesn't mean the hashed addresses are globally valid (they could be within multicast, for example).
* The last IPv4 /24 or IPv6 /64 can still be correlated, allowing you to pick out relatively similar hashed addresses.  For example, if you see both 230.10.134.107 and 230.10.134.226 hashed addresses, you know they're part of the same /24, even though you know nothing else about the addresses.
* However, the last IPv4 /24 or IPv6 /64 is not globally hashed and does not leak information about itself.  If you happen to know hashed 230.10.134.107 corresponds to real 10.9.8.4, hashed 178.59.91.107 does not mean the real IP ends in .4.
* A hash key is optional but recommended.  Reuse the key if you want to be able to correlated hashed IPs over time, or be able to hash a known real IP to search for it in the past.  Use a random key and throw it away afterwards if you want it to be truly anonymous.
* SHAKE-256 is used because it's modern (SHA-3) and support arbitrary-length output. If you can't use SHA-3, use something like SHA-512, HMAC it with a key, and use a portion of the output. (SHA-3 digests can securely use key prepending; other digests require HMAC.)
* Hashed IPv6 addresses get quite unwieldy. Get used to addresses like 20f1:b413:7f5d:7720:1fe2:1bf3:38fb:620. (Exactly one hex byte out of 32 was compressible in that random example I used. Most examples have none.)
