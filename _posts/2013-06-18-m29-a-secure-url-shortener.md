---
author: Ryan Finnie
categories:
- Uncategorized
date: 2013-06-18 02:01:54
guid: http://www.finnie.org/2013/06/18/m29-a-secure-url-shortener/
id: 2273
layout: post
permalink: /2013/06/18/m29-a-secure-url-shortener/
tags:
- planet:canonical
title: M29, a secure URL shortener
---
A year ago, I launched [M29](http://m29.us/), a URL shortener with a twist. Apparently I forgot to announce it here. Whoops.

Normal URL shorteners are fairly simple. You submit a long URL. The service generates a short URL. The long and short URL are placed in a backend database. If you go to the short URL, it redirects to the long URL.

This means that the URL shortener service has a large database of URLs available to it. While 99% of the contents of this database may be mundane, it's still a large, centralized source of information. Very relevant to the recent NSA news, for example.

M29's twist is, except when serving the redirect, it does not know anything about the contents of the long URLs. This is accomplished by generating an AES-128 key, using it to encrypt the long URL, and then splitting the key in two. One half of the key is stored in the backend service, and the other half is encoded as part of the short URL itself. This means the only time the two parts of the key come together is when the short URL is requested for the redirect.

Getting from a long URL to a short URL can be done one of several ways. If you go to m29.us and have Javascript enabled, the client side actually loads an AES libary, builds the key, encrypts the URL, and sends the encrypted URL and half of the key to the server, all processed on the client side. If you don't have Javascript enabled, this task is farmed out to the server side, which generates a random key, encrypts, makes the database insert, returns the short URL, then throws away half of the key. M29 also has a featureful [API](http://m29.us/api/) which lets you do these tasks yourself. (It is also compatible with the [goo.gl API](https://developers.google.com/url-shortener/), which is easy to work with and has several tools available.)

The net effect is, while I currently have a database of about 10,000 entries, I cannot read them. Source IP and URI logging are not done on the server, so the only way I can find a long URL is if I load a full short URL, which is not possible given just the backend database.

Anyway, this weekend I did some work on M29, including adding a [bit.ly-style preview option](http://m29.us/AQ/UN3yIzB_FrI+) (append a "+" to the short URL to get its info), among other small feature additions and fixes. It was then I realized, by going to that above short URL (the first URL generated and used in documentation) that the one-year anniversary of the service is today.
