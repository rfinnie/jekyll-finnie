---
date: 2009-07-18 15:00:27-07:00
layout: post
title: X11R5 Joins Twitter, Starts Harassing Oprah
wp_id: 952
---
<div style="float: right;">
  <a href="https://www.flickr.com/photos/fo0bar/3587751864/" title="X11R5 on the street by Ryan Finnie, on Flickr"><img src="https://farm4.static.flickr.com/3641/3587751864_d926a09af9_m.jpg" width="240" height="180" alt="X11R5 on the street" /></a>
</div>

[Last month](http://www.finnie.org/2009/06/19/beep/), I briefly introduced you to [X11R5](http://identi.ca/x11r5), a [Markov bot](http://en.wikipedia.org/wiki/Markov_chain) on [Identica](http://identi.ca/).

X11R5's history actually goes back a few years, as an IRC bot. He learns from chatter on a few select IRC channels, responds to direct queries, and when the channels are idle, he occasionally blathers randomly. Those blathers are recorded in a database, and are used to power [x11r5.com](http://www.x11r5.com/), including an [RSS feed](http://www.x11r5.com/x11r5.rss) that was then simulcast to [Twitter](http://twitter.com/x11r5) and [Identica](http://identi.ca/x11r5).

However, the Twitter and Identica feeds didn't prove to be very popular. He wasn't interactive, and people tended to ignore him.

A few months ago, I split out the random collection of Perl scripts that powered X11R5, collectively known as Cybersauce, and adapted them to use with Identica directly, as opposed to being a by-product of the IRC X11R5. (The founders of Identica are friends and acquaintances, so I've been there from the start, and Identica is generally more pleasant and less teen-angsty than Twitter.) I needed material to train the Markov brain, and ended up getting an anonymous dump of the last 50,000 notices from [@evan](http://identi.ca/evan). In addition, a script will occasionally pull posts from the front page [public timeline](http://identi.ca/) in order to continually train.

Other scripts allow X11R5 to blather; to @ reply to people who have subscribed to him and have posted recently; to @ reply to people who have @ replied to him; to occasionally favorite notices; and even to occasionally @ reply to strangers (this is the functionality that seems to generate the most interest and get the most subscribers). X11R5 does all of this throughout the day.

The "new" X11R5 on Identica has been a rousing success, and has developed a personality completely distinct from the IRC X11R5. He's even fluent in multiple languages! ([English](http://identi.ca/notice/6547151), [German](http://identi.ca/notice/6528406) and [Portuguese](http://identi.ca/notice/6526162) seem to be the most common.) He occasionally causes some controversy (for example, there is a very small chance that he will ["re-tweet" a Markov'd version of someone's notice](http://identi.ca/conversation/6334633#notice-6335607)), but this almost always dies down when the victim realizes what he is. They usually even end up subscribing to him.

With this success, I added Twitter functionality. Again, he has a brain separate than the IRC or Identica brains. To get training material, I had to learn directly from the public timeline. However, Twitter API access is rate limited, and I was effectively limited to learning 5 notices per minute (a large majority of notices have to be thrown out due to spam, etc). I applied for rate limit whitelisting for Cybersauce, and was approved. With that, I was able to get enough training material in about a day. (NB: That day was Canada Day, which is why he mentions it occasionally.)

X11R5 on Twitter has been running for a few weeks, and he's become... [well, a 13 year old](http://twitter.com/x11r5/status/2669607852). Again, a distinct personality has emerged because of the different class of training. Maybe I should write a paper. I've also noticed that this X11R5 is less fun to follow than the Identica version, primarily because Twitter lacks the [public reply](http://identi.ca/x11r5/replies) and [threading functionality](http://identi.ca/conversation/6205701#notice-6212932) that Identica has. Still, he's gathered some interest.
