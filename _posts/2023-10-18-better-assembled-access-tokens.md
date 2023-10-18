---
date: 2023-10-18 15:15:57-07:00
excerpt: Better-Assembled Access Tokens (BAAT) is an open specification for generating
  tokens with self-identification and verification properties
excerpt_standalone: false
layout: post
tags: []
title: Better-Assembled Access Tokens
---
A few years ago, [GitHub changed the format of their access tokens](https://github.blog/2021-04-05-behind-githubs-new-authentication-token-formats/) from a hexadecimal format which was indistinguishable from a SHA1 hash, to a format with a human-identifiable prefix and built-in checksumming, which can be identified as a GitHub token by a program. This is useful for being able to determine if, for example, an access token was accidentally committed into a repository. I welcomed this, but recently wanted to build an agnostic version which could be used in other systems.

Enter: Better-Assembled Access Tokens (BAAT). The token format looks like so:

```
bat_pfau4bdvkqwmwwur2bjo2q2squjeld5fafgyk5sd
bat_3udmmr57bglierumrjxjxrkiv3nydd5faebohhgn
bat_bbzz6q4rnbnu6tkujrb73vhfuk6pdd5fafme5kq5
```

"bat" is the prefix and can be any lowercase alphanumeric string, but should be between 2 and 5 characters.

The other part -- the wrapped data -- contains a payload of 144 bits (18 bytes), a magic number and version identifier, and a checksum. This payload size allows for a full UUID to be generated, with 2 bytes left for additional control data if needed.

The checksum includes all of the data, including the prefix (which is not a feature of GitHub's tokens), and the fact that it has a binary magic number means a BAAT can be identified programmatically, no matter the prefix chosen by the application. A BAAT is canonically all lowercase, but can handle being case-corrupted in transit.

A sample Python implementation is below, but the general specification for BAAT is:

- If the binary payload is under 18 bytes, pad it to 18 bytes
- CRC32 the prefix + payload + magic number (`\x8f\xa5`) + version (`\x01`)
- Assemble the wrapped data as a base32 concatenation of payload + magic number + version + CRC
- Assemble the final BAAT as the prefix + "_" + the wrapped data

And to verify a BAAT:

- Split the string into prefix and wrapped data by "_"
- Base32 decode the wrapped data and verify it's at least 7 bytes
- Verify the 2 bytes at position -7 is `\x8f\xa5`
- Verify the byte at position -5 is `\x01` for version 1 (currently the only version, but doesn't hurt to future-proof -- the rest of the process assumes a version 1 BAAT)
- Verify the wrapped data is 25 bytes
- Extract the payload as the 18 bytes at position 0 (the beginning), and the checksum as the 4 bytes at position -4 (the end)
- Verify the checksum as the CRC32 of prefix + payload + magic number + version

This specification is open; feel free to use it in your implementations!

If you're wondering why the magic number and version are in the middle of the wrapped data instead of the front like it normally is for a data format (thus requiring some additional positional math), it's because it shows up as a static sequence of text in a list of multiple BAATs. Placing the payload at the beginning and the checksum at the end allows a human to quickly pattern match "oh, this is the '3ud' token, not the 'pfa' token".

If you're wondering why the payload is 18 bytes, it's because BAAT use base32 for the encoding, which will use trailing equal signs as padding. 20 input bytes is a multiple with no padding, which would have allowed for a 16-byte payload and 4-byte checksum. But I wanted to have a 2-byte magic number, and the next multiple without padding was 25 bytes, so the final 3 bytes were used for a 1-byte version and 2 extra payload bytes.

```python
# Better-Assembled Access Tokens
# SPDX-FileCopyrightText: Copyright (C) 2023 Ryan Finnie
# SPDX-License-Identifier: MIT

from base64 import b32decode, b32encode
from random import randint
from zlib import crc32


class BAATError(ValueError):
    pass


def make_baat(prefix="bat", payload=None):
    magic = b"\x8f\xa5"
    baat_ver = b"\x01"
    if payload is None:
        payload = bytes([randint(0, 255) for _ in range(18)])
    elif len(payload) > 18:
        raise BAATError("Payload too large")
    elif len(payload) < 18:
        payload = payload + bytes(18 - len(payload))
    prefix = prefix.lower()
    crc = crc32(prefix.encode("utf-8") + payload + magic + baat_ver) & 0xFFFFFFFF
    wrapped_data_b32 = b32encode(payload + magic + baat_ver + crc.to_bytes(4))
    return (prefix + "_" + wrapped_data_b32.decode("utf-8")).lower()


def parse_baat(baat):
    parts = baat.split("_")
    if len(parts) != 2:
        raise BAATError("Malformed")
    prefix = parts[0].lower()
    wrapped_data = b32decode(parts[1].upper())
    if len(wrapped_data) < 7:
        raise BAATError("Impossible length")
    magic = wrapped_data[-7:-5]
    baat_ver = wrapped_data[-5:-4]
    if magic != b"\x8f\xa5":
        raise BAATError("Invalid magic number")
    if baat_ver != b"\x01":
        raise BAATError("Invalid BAAT version")
    if len(wrapped_data) != 25:
        raise BAATError("Wrong length")
    payload = wrapped_data[0:18]
    crc = crc32(prefix.encode("utf-8") + payload + magic + baat_ver) & 0xFFFFFFFF
    if wrapped_data[-4:] != crc.to_bytes(4):
        raise BAATError("Invalid CRC")
    return payload


def is_baat(baat):
    try:
        parse_baat(baat)
    except ValueError:
        return False
    return True


if __name__ == "__main__":
    payload = bytes([randint(0, 255) for _ in range(18)])
    baat = make_baat("bat", payload)
    print(baat)
    parsed_payload = parse_baat(baat)
    assert is_baat(baat)
    assert parsed_payload == payload
```
