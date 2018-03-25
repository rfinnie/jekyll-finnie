---
date: 2018-03-25 16:54:43-07:00
excerpt: Wherein Ryan plays with Let's Encrypt wildcard certificate registration
excerpt_standalone: true
layout: post
title: Let's Encrypt wildcard certificates
---
The [vsix.us IP tests site](https://vsix.us/) is now fully HTTPS.  [About a year ago](https://www.finnie.org/2016/12/18/sure-lets-encrypt/) I converted all the sites I could over to HTTPS with [Let's Encrypt](https://letsencrypt.org/), and while vsix.us itself was converted to HTTPS, the "nocache" wildcard site remained at HTTP because Let's Encrypt did not have wildcard support at the time.

About a week ago [they announced wildcard support](https://community.letsencrypt.org/t/acme-v2-and-wildcard-certificate-support-is-live/55579), and today I registered *.nocache.vsix.us.  The process wasn't seamless, and this blog post is meant to be some notes on getting it working, not necessarily a guide.

Let's Encrypt only supports wildcard registration via the ACME v2 protocol and dns-01 validation.  I'm not exactly sure why it's dns-01 only, as they are not checking multiple subdomains, just using the TXT record of _acme-challenge.example.com to validate *.example.com.  Unless I'm missing something, http-01 validation on example.com would be just as secure.

In my case, this means configuring BIND for secure dynamic updates.  [The documentation for certbot-dns-rfc2136](https://github.com/certbot/certbot/tree/master/certbot-dns-rfc2136) is straightforward, but complicated by the fact that my zones are DNSSEC signed, so BIND itself needs to be able to re-sign the zone on the fly.  (In my case, I re-sign the zones myself after making zone updates.)  Ultimately I split out nocache.vsix.us to its own zone and configured BIND so it could re-sign upon update.

`certbot` needs to be at least version 0.22 to support ACME v2, needed for wildcard.  The Ubuntu PPA includes 0.22.2 as of this writing, but it does not include `certbot-dns-rfc2136` needed for dns-01 validation.  But it's relatively easy to install manually:

```
git clone https://github.com/certbot/certbot
cd certbot/certbot-dns-rfc2136
sudo pip3 install .
```

Also of note is 0.22 still defaults to ACME v1.  You will need to pass the v2 endpoint via `--server`, but keep in mind that the v2 endpoint is essentially a completely different service, and it will once again ask registration questions (email address, etc).  With all that in place, here was the final invocation for me:

```
certbot certonly \
  --server 'https://acme-v02.api.letsencrypt.org/directory' \
  --dns-rfc2136 \
  --dns-rfc2136-credentials /etc/letsencrypt/dns-01.ini \
  -d nocache.vsix.us -d '*.nocache.vsix.us'
```