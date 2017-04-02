---
date: 2017-04-02 13:02:34-07:00
excerpt: Wherein Ryan tells the Certificate Authorities who is permitted to speak
  for him.
excerpt_standalone: true
layout: post
title: SSL certificates and DNS CAA
---
Late last year, [I converted all of my web sites to SSL with Let's Encrypt](https://www.finnie.org/2016/12/18/sure-lets-encrypt/).
Since then, I've made sure other SSL-enabled services have proper trusted certificates, such as my mail server and XMPP server.
Historically, those types of services, when SSL enabled, tended to be self-signed certificates.
For example, while MX mail servers on the Internet often are TLS enabled, trust is almost never checked or enforced because it is assumed few people or organizations would pay extra money for a non-web service.
And in the cases of IMAP, XMPP, etc, the end user will often just click "accept and ignore" once and that'll be the end of that.
Trust chains were largely relegated to the web, but since Let's Encrypt makes this free and relatively easy, I can see that changing.

It's been more than 3 months, so I'm also happy to say my automated processes for renewing certs worked without a hitch.
One of the only annoyances about Let's Encrypt is the certificates are only valid for 3 months, so you need to rely on automated renewal and installation of the updated certificates.

My web sites are now secure and trusted, but what's to stop a certificate authority from issuing an SSL certificate for, say, finnie.org without my permission?
A few things, actually.
At the very least, CAs perform Domain Validation (DV), usually involving giving the requestor a token to place on the web site or in DNS.
This lets the CA know that the person requesting the certificate at least has access to the web site or DNS for a domain.
Other methods (usually at a higher cost) include Organization Validation (OV) and/or Extended Validation (EV, the "green bar" certificates), which involves the CA doing more manual investigation than DV.
Make sure the domain is really an asset of the organization, make sure the person is a member of the organization and is authorized to request on behalf of the organization, etc.

So there are a number of procedures to place trust in the certification path, but most rely on the procedures of the CA itself.
This compliance is kept in check by the cabal of browser vendors (Firefox, Google, Microsoft, Apple) which require auditing and compliance in exchange for including the CA's certificate in the browser's trusted root store.
And having your root certificate removed is essentially a mark of death (see [WoSign/StartCom's woes last year](https://blog.mozilla.org/security/2016/10/24/distrusting-new-wosign-and-startcom-certificates/) which led to their roots being distrusted by all the major browser vendors, and [Symantec's in-progress woes with Google](https://groups.google.com/a/chromium.org/forum/#!topic/blink-dev/eUAKwjihhBs)), so in theory, the CAs are motivated to keep honest.

But there is one layer of assurance you can add yourself, a new DNS record called [Certification Authority Authorization](https://en.wikipedia.org/wiki/DNS_Certification_Authority_Authorization), or CAA.
I've added CAA records to all of my domains, and they look like so:

<pre>finnie.org.  3600  IN CAA  0 issue "letsencrypt.org"
finnie.org.  3600  IN CAA  0 issuewild "letsencrypt.org"
finnie.org.  3600  IN CAA  0 iodef "mailto:&lt;redacted&gt;"</pre>

Essentially, this says "Let's Encrypt is the only certificate authority allowed to issue a certificate for finnie.org."
When I register or renew a certificate with Let's Encrypt, it checks the CAA records and verifies letsencrypt.org is a permitted issuer before performing additional checks.
If CAA records are present but do not include letsencrypt.org, the issuance request is denied.
Similarly, if a third party goes to, say, DigiCert and requests a certificate for finnie.org, the request is immediately denied.
In that case, the iodef entry is an email address where someone can be notified if such an attempt is made, but as of now, I'm not aware of any CAs which utilize automated notification.

Note that there are two entries, "issue" and "issuewild", both set to letsencrypt.org.
At this time, Let's Encrypt does not issue wildcard certificates, but remember these entries are basically for everybody *but* the CA(s) mentioned.
Let's Encrypt may not issue wildcard certificates today, but I don't want another CA to issue a wildcard cert for my domain.

Also of note is that these records are solely for the CAs to use when a certificate is being issued.
It is not intended for, say, a browser to verify a certificate in real time.

CAA is definitely an additional layer of protection, but it still relies on the CAs utilizing and adhering to it.
There's nothing to prevent me from creating my own certificate authority and issuing a certificate for google.com, but that would be of little use as my CA root isn't trusted anywhere.
As of today, [only about half the major CAs check CAA](https://sslmate.com/labs/caa/), but in March [a vote was taken by the CAs](https://cabforum.org/pipermail/public/2017-March/009988.html) which mandates its use as of September 2017.
As I understand it, this is not a requirement that end user requesters must have CAA entries when requesting a certificate, just that CAs must check CAA and act appropriately if the records are present.

So now we have an extra layer of DNS-based protection, but DNS itself is a fairly insecure system.
What can we do about that?
Coming soon: DNSSEC.
