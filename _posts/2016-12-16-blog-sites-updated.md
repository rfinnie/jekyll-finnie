---
date: 2016-12-16 11:40:14-08:00
excerpt: Wherein Ryan blogs about blogging, and has been on the blogosphere since
  back when it was called the interblog
layout: post
title: Blog sites updated
---
I've been on the ol' blogosphere for over 15 years now, starting with a LiveJournal back in 2001.
In 2009, I moved the contents of the LiveJournal to [my personal site](http://www.finnie.org/), a WordPress installation.
In addition, I've got a blog for [Finnix](http://www.finnix.org/) at [blog.finnix.org](http://blog.finnix.org/), also a WordPress site.

I haven't been blogging much (my last post was a little over a year ago), but I've been keeping the software up to date on them, as WordPress is not exactly know to be the most secure⁰.
However, I've had a thought in the back of my mind for awhile:
Why not convert the posts to [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet), write up a template and serve them statically?
Turns out a decently generalized solution already exists for that, [Jekyll](https://jekyllrb.com/).

Over the last week, I've been converting both sites over from WordPress to Jekyll.
The layout is simple, mostly baseline [Bootstrap](http://getbootstrap.com/), but it's starting to reach the point where it takes a lot of effort to make it look like something which didn't take a lot of effort.
One of the nice things about Jekyll is it's all just flat files, so it's easy to track with Git.
I've got repositories for both sites on GitHub ([finnie.org](https://github.com/rfinnie/jekyll-finnie), [blog.finnix.org](https://github.com/rfinnie/jekyll-finnix)), if you want to take a look.
The templates would mostly work as a generic base, but do have some personal customizations (such as the Google site search at the top of finnie.org).

For my personal site, I also did some pruning of old content.
When I migrated to WordPress in 2009, I imported _all_ of my LiveJournal posts.
Personal LiveJournal posts tend to have a different "feel" than content-focused blog posts, and let's be honest, there was a lot of angsty 20-something posting I did throughout the 2000s I'm a bit embarrassed by now.
There were over 900 posts on my personal WordPress site.
I saved all of my posts since the conversion to WordPress in 2009, plus a handful of pre-2009 LiveJournal posts I wanted to keep because they were interesting or important.  Interestingly, nothing from the first 2½ years survived, but in total, I picked a little over 200 posts to keep.

--

⁰ One might blame that on being PHP, and while it's true that PHP makes it very easy to shoot yourself in the foot, it doesn't have to be that way.
The classic example I give of a good PHP project is [MediaWiki](https://www.mediawiki.org/).
It's a large open source project with a decent track record for security, and it has a very good development contribution model.
(It was one of the first large projects to embrace the open contribution model.
Fork the repository, make a change, push it to a personal branch, open a merge request, it gets tested against CI, and core developers can then merge it directly.
Back in the day it was an eye opener to go, "Wait I don't have to be a project developer to make a commit directly?")

