---
id: 2061
title: OpenPGP key transition
date: 2012-04-11T10:53:39+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/2012/04/11/openpgp-key-transition/
permalink: /2012/04/11/openpgp-key-transition/
categories:
  - Uncategorized
tags:
  - planet:canonical
---
A copy of this announcement is available at <http://www.finnie.org/rfinnie-openpgp-2012-transition.txt>, in case the text is mangled here and the signature cannot be verified.

<pre>-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA256,SHA1

Wed, 11 Apr 2012 10:30:08 -0700

For a number of reasons, I've recently set up a new OpenPGP key, and 
will be transitioning away from my old one.  My old key was created 
over 10 years ago, as a 1024 bit DSA key with a SHA-1 signatures, both 
of which are considered inadequate today.  My new key is a 4096 bit RSA 
key with SHA-256 signatures.

The old key will continue to be valid for at least 90 days.  It will be 
revoked on or around 2012-07-15, or after the release of Finnix 105, 
whichever is later.  (My old key was used to manage signatures for the 
Finnix project.  This will be split out into a Finnix-specific signing 
key, and will be announced in a separate message.)

However, I would prefer all future correspondence to come to the new 
one, as of today.  I would also like this new key to be re-integrated 
into the web of trust.  This message is signed by both keys to certify 
the transition.

The old key was:

pub   1024D/203ECA25 2001-05-09
      Key fingerprint = B023 7C63 DF28 70AA C3AB  C54A 2996 10A9 203E CA25

And the new key is:

pub   4096R/86AE8D98 2012-04-11
      Key fingerprint = 42E2 C8DE 8C17 3AB1 02F5  2C6E 7E60 A3A6 86AE 8D98

To fetch the full key (including a photo UID, which is commonly
stripped by public keyservers), you can get it with:

  wget -q -O- http://www.finnie.org/rfinnie.gpg | gpg --import -

Or, to fetch my new key from a public key server, you can simply do:

  gpg --keyserver pgp.mit.edu --recv-key 86AE8D98

If you already know my old key, you can now verify that the new key is
signed by the old one:

  gpg --check-sigs 86AE8D98

The new and old keys' primary UIDs are both "Ryan Finnie 
&lt;ryan@finnie.org>".  This was by design, to ensure you must verify the 
key signatures rather than seeing something like "Ryan Finnie (2012) 
&lt;ryan@finnie.org>".

If you don't already know my old key, or you just want to be double
extra paranoid, you can check the fingerprint against the one above:

  gpg --fingerprint 86AE8D98

If you are satisfied that you've got the right key, and the UIDs match
what you expect, I'd appreciate it if you would sign my key:

  gpg --sign-key 86AE8D98

Lastly, if you could upload these signatures, I would appreciate it.
You can either send me an e-mail with the new signatures (if you have
a functional MTA on your system):

  gpg --armor --export 86AE8D98 | mail -s 'OpenPGP Signatures' ryan@finnie.org

Or you can just upload the signatures to a public keyserver directly:

  gpg --keyserver pgp.mit.edu --send-key 86AE8D98

Please let me know if there is any trouble, and sorry for the
inconvenience.

Thank you,
Ryan Finnie

[Much of this text was adapted from dkg &lt;http://fifthhorseman.net/>,
thank you!]
-----BEGIN PGP SIGNATURE-----
Version: GnuPG v1.4.11 (GNU/Linux)

iQIcBAEBCAAGBQJPhcEbAAoJEH5go6aGro2YCqYQAKM2IlO3CgOLPDYIww7tdt0t
TTYgp1ng0oOkRdSKm7maavnVd8Drkys/TgO8DQD/tuf37ZES1Vid7yqQSddAx49/
da+V9EdbCaZOqqVUY0qtW5JTV8xyn67zLwhj06/L+NWf3iP/6ymCzbWrVor2jdtn
Efeylj+T+j5igLOTBkx22d4W3VU787fiMCiwLgDmytwJ66cHR4qR+jWWnsEdVuuF
AVwcs9ELRicppE0p1jMmsr/rKJJAeM0xb1+V+BL685q4XkXRvY6Fg2WC2aoTFJF/
jp94JtlodooWuCuWnNFofqVdYIuSezjki+aRy3KmCFliWaULqL8akdtVlUmA/2gM
PdZE7Acf7JU4TVH/drvY6pbK7zwFIuBA+ESbB4lJEvZFC+Ub2aM7SceDAp2CBd+i
B4+sWkv89ZSDZqGXK2ylTNFU2o2MfQLxZWKZOdq0exZJkb5NSNF22YY8WsMsXpqJ
Ydtt0mxVp57rkhc01Vx4DJ5+OKmCJEgiTj+wnef1RvZh3ayLqkS5wUTkf6S4OLwP
cJT3i+mhAU7CQVFqSnmg98ADiq1SVnWz2rsq4m1e965ST1OpNxicK4g9UO/ePUT2
yBtyEfmFCV98KCADUSdWmD0Nx3uzHxtb+0RMMulPOQszB9VDPxIcNbdcKMLzzcp+
ZwM/dc405Tvdzptf/khgiEYEARECAAYFAk+FwRsACgkQKZYQqSA+yiXbTwCggR1l
9IHQVOKCDEJmot02C8pRFFIAnjvSY/eCeLW3mjvBF8rQUCg80KRJ
=pweu
-----END PGP SIGNATURE-----</pre>
