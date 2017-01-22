---
date: 2017-01-21 19:43:24-08:00
excerpt: Wherein Ryan blogs about blogging, again
excerpt_standalone: true
layout: post
title: A brave new world of blogging
---
About a month ago, [I converted my blogs over to Jekyll](https://www.finnie.org/2016/12/16/blog-sites-updated/).
(I've also made the leap and converted all of my web sites over to HTTPS, and upgraded my main colo box from Ubuntu 14.04 to 16.04, taking the opportunity to retire a ton of old PHP code during the upgrade from PHP 5 to PHP 7.
It's been a busy few weeks.)

Part of the process of converting the blogs to Jekyll was storing the site data and posts in Git repositories.
This was mostly for my own benefit, and I certainly could have kept the repositories private, but instead chose to push them to GitHub ([finnie.org](https://github.com/rfinnie/jekyll-finnie), [blog.finnix.org](https://github.com/rfinnie/jekyll-finnie)) to serve as examples for others who may wish to do the same thing.

At the bottom of each page, I do have a small "Git source" link to the corresponding page on GitHub.
I didn't expect anyone would really notice, and I certainly did not expect anyone to make a pull request against the repositories.
So I was surprised when I saw [this pull request](https://github.com/rfinnie/jekyll-finnie/pull/1), by someone who fixed the formatting on [one of my imported posts](https://www.finnie.org/2010/03/07/external-temperature-monitoring-with-linux/).

Unexpected, but certainly welcome!
So feel free to open Issues or PRs against those repositories, but I would discourage you from actually writing new posts and asking for them to be published via a PR.

As for writing posts, editing was an unexpected complication.
While I spend most of my day in a terminal editor[0] which is fine for writing code and the occasional documentation, I found I wasn't comfortable doing free-form writing there.
I've simply been used to writing blog posts in a web browser for over 15 years (LiveJournal, then Wordpress), with instant spell checking and a relatively quick preview available.
While I've got a few things I want to write about, I found myself hesitating with the actual writing because of the editor situation.

I went looking for an editor I could use just for Markdown blog post writing, and eventually came across [ReText](https://github.com/retext-project/retext).
It's primarily a Markdown editor which is good for this purpose, has built-in spell checking, Markdown highlighting, a dropdown list of common Markdown formatting, and most usefully, allows for side-by-side editing and live preview.
This is the first post I've written completely with ReText, and while it's not perfect, it's made it easier for me to *just write*.

[0] Most of the time it's nano, if you must know.
Yes, get it out of your system.
I've been using nano née pico since my first days on "the Internet", a 1992 dialup BSD VAX account which had the full UW suite.
So yes, I've been using nano since before some of you were born.