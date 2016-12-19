---
date: 2016-12-18 21:47:19-08:00
layout: post
title: Sure, Let's Encrypt!
---
This weekend I converted all of my web sites to SSL (technically TLS), using [Let's Encrypt](https://letsencrypt.org/) certificates.

For a few years, I had been using [StartCom's free SSL cert](https://www.startssl.com/) for a non-essential web site on my main server.
StartCom was effectively the only viable free certificate authority at the time, with good browser support.
[CAcert](http://www.cacert.org/) wasn't too useful since almost nothing ships with its CA certificates, while [Comodo](https://ssl.comodo.com/free-ssl-certificate.php) has free SSL certs which expire after 90 days, essentially a free demo.

I had been planning on tackling global SSL since earlier this year.
The biggest hurdle is adoption of Server Name Indication, or SNI.
Before SNI, you were limited to one SSL certificate per IP address, much like the time in the 90s before name-based virtual hosts.
And like name-based virtual hosts, even after the technology was developed, it was slow to be adopted since name-based vhosts and SNI both require the web client to have support for them.

I had set a self-imposed delay until April 2017, with the 5-year EOL of Ubuntu 12.04 LTS (precise).
While overall, this was a good round number to stick to, the technical reason was <kbd>wget</kbd> had not yet added SNI support until just after precise was released.
After that, remaining incompatibilities would be at an acceptable level (basically just Windows XP).

This weekend I was playing around, and ordered a few new StartCom certs as a test for a few sites ([finnie.org](https://www.finnie.org/), [vad.solutions](https://vad.solutions/)), with the idea of setting up dual HTTP/HTTPS, and not switching over to HTTPS only until 2017.
Things were working well, until I noticed when the iPhone browser tried to connect, it immediately dropped the connection.
Likewise, when trying with Safari on a Mac, it came back with "This certificate has an invalid issuer".
Examining the cert path side-by-side with the old site (with a cert from February), both the old site and the new sites had identical issuer certificates, but Apple browsers were not trusting the new certs.

I spent hours trying to figure this out, thinking it must be an SNI-related server-side issue.
Eventually I came across a [post by Mozilla Security](https://blog.mozilla.org/security/2016/10/24/distrusting-new-wosign-and-startcom-certificates/) which explained the issue.
Apparently StartCom Did a Bad (backdated cert issuances and lied about its organizational structure), and got the mark of death by the major browser vendors.
However, the way the vendors did that was by continuing to trust issued certs before a certain date a few months ago, but anything after that is not trusted, which explains why my old site continued to work.
Firefox and Chrome worked fine with the new certs, but that's because their change has not yet hit released products yet, while Apple's been quicker on the matter.
(This also explains why StartCom has gone from free 1-year certs to free 3-year certs: they're essentially worthless.)

So I started looking into Let's Encrypt.
I had known about the project since it was launched last year, but didn't give it much thought until now.
Primarily, I thought the project wouldn't be too useful as a new CA would take years to get into the major browsers and OSes to the extent it would be viable.
Also, it had sounded like I needed to run an agent which took over configuration of Apache for me, something I did not want to do.

Turns out I was wrong on both fronts.
Their intermediary signing cert is actually cross-signed by an established CA (IdenTrust), so even if the browsers and OSes don't have the Let's Encrypt root CA, certs they sign will still be trusted.
On the software side, you still do need to run an agent which handles the verification and ordering, but it doesn't need to completely take over your web server.
You can run it in "certonly" mode and point it at a web site's root (it needs to be able to place a few challenge/response files there for verification), but lets you handle the resulting certificate.
The certs are only valid for 90 days, but the idea is you idempotently run the renew command from cron daily, and it'll seamlessly renew the certificates.

Let's Encrypt has since become the world's largest certificate provider, by raw number of certificates.
And since their cry is "convert all your sites over to SSL", the implicit implication is SNI is here and we don't need to worry too much about older clients.
So hey, may as well get going now!

Most sites were trivial to convert.
For example, [74d.com](https://74d.com/) has nothing except "hey, maybe some day someone will pay me an insane amount of money for this domain".
Other sites required more planning.
The biggest problem with starting to serve an existing site via HTTPS is all images, CSS and JS also needs to be served via HTTPS, even if the domain is configured to completely redirect from HTTP to HTTPS.
[finnie.org](https://www.finnie.org/) is a deceptively massive site, with decades worth of junk layered on it (the domain itself is actually coming up on its 20 year anniversary in April), so it took a lot of grepping for places where I had hard-coded HTTP content.
I'm sure I've missed places, but most of it has been fixed.

[finnix.org](https://www.finnix.org/) is another example of requiring a lot of thought.
The main site itself, a mostly self-referential MediaWiki site, was easy to do.
But it also has about a dozen subdomains which required individual examination.
For example, [archive.finnix.org](https://archive.finnix.org/) is a static site which just serves files; trivial to redirect to HTTPS, right?
Problem is, they are mostly fetched by <kbd>apt</kbd>, which 1) does not follow HTTP redirects, and 2) does not support HTTPS unless an additional transport package is installed.
So if I switched that to HTTPS only, it would break a number of Finnix operations.
In the end, I decided on serving both HTTP and HTTPS, and setting it up so if you go to [http://archive.finnix.org/](http://archive.finnix.org/) it'll redirect to [https://archive.finnix.org/](https://archive.finnix.org/), but individual files can be retrieved via either HTTP or HTTPS.

In total, I've created 22 certificates, covering 41 hostnames.
As Let's Encrypt is now essentially the only free CA, I wish them well, and have even [donated $100 to their non-profit](https://www.generosity.com/community-fundraising/make-a-more-secure-web-with-let-s-encrypt).
This is really putting all my eggs in one basket; once you go to HTTPS-only for a site, it's very hard to go back.

**Edit**: It's been pointed out that you can get around SNI issues by using multiple Subject Alternative Names (SANs) with Let's Encrypt, as SAN has much more older support than SNI current does.
I had been using SANs for multiple similar hostnames on a certificate (e.g. www.finnie.org had a SAN for finnie.org), but thought certbot required them all to have a single document root.
Turns out you can define a "webroot map" for multiple hostnames to multiple document roots, and there are no defined limits to the number of SANs you can use (though it appears the accepted effective limit in the industry is about 100).

The big downside of one cert with multiple SANs is you are now publicy advertising a group of which sites you are administering, but in this case I'm fine with that.
I've changed things so 37 of my hostnames are now covered under one certificate.

Also, the part about Ubuntu precise's wget not supporting SNI is no longer correct.
Thankfully SNI support for wget had been backported to precise in May 2016.
