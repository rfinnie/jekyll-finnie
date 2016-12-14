---
author: Ryan Finnie
categories:
- Uncategorized
date: 2006-04-16 14:40:00
guid: http://www.finnie.org/2006/04/16/thoughts-on-my-new-cellphone/
id: 580
layout: post
lj_import_url:
- http://fo0bar.livejournal.com/150754.html
lj_itemid:
- '150754'
permalink: /2006/04/16/thoughts-on-my-new-cellphone/
title: Thoughts on my new cellphone
---
I've had my [Cingular 8125](http://www.cingular.com/8125_consumer) for a few days, and here is what I have to report so far:

**Slide-out keyboard:** awesome. When you slide out the keyboard, the display automatically switches to landscape mode. While of course not as fast as a normal keyboard, it's still much quicker to type than any other method I've seen for cellphone/PDA input. The biggest drawbacks so far are the lack of Escape, Alt, and Page Up/Page Down keys (and Ctrl, but that's available on the popup on-screen keyboard), which makes irssi in [PocketPuTTY](http://www.pocketputty.net/) difficult. I have compensated for this by defining /u and /d to scroll up and down in the history, and I've learned to use "/win 5" instead of Alt-5 like normal.

**Battery life:** decent. Probably about 5 days if I weren't playing with it all the time.

**Downloaded applications:** I downloaded an RSS reader that displays headlines on the "today" page, and set it to update daily at 8AM. Also installed are PocketPuTTY, Opera, Microsoft Voice Command, and a NTP utility (why most Cingular phones don't sync against the GSM time service is beyond me).

**Exchange:** I fully expect shit from the peanut gallery, but I'm loving the integration with our Exchange server at work. Email (last N days worth, I have it set to 3), calendaring, tasks and contacts are all integrated with my exchange account, and of course the phone's phonebook uses the contact list. Because of this, I'm actually starting to use the exchange features. I typed the contacts from my last phone into Outlook on my work desktop, and they now show up on the phone. I've found that when I remember something I need to do (mostly for work, but personal stuff too), I just slide out the keyboard, select "tasks", and type it in. And I'm starting to enter stuff in my calendar as well.

I've got it set up so when it's docked at my desktop at work, it syncs every 5 minutes. When not docked, it syncs every 15 minutes over-the-air during peak hours, and every 4 hours off-peak. Note: This just affects Exchange stuff. SMS messages still arrive real-time.

**Voice command:** awesome. Just press a button on the side of the phone, say a command, and it does it. "Call voicemail." "Dial 555-1212." "What time is it?" "What is my next appointment?" "Open solitare." "Play genre 80s." This utility is real speech recognition, as opposed to some phones where you have to record commands and it just compares your command against commands you recorded earlier.

One thing I have to remember is the difference between "call" and "dial". You "call" a person, but you "dial" a number. This happened yesterday: "Dial John [last name that sounds like 'eleven']" "Dial 9-1-1. Correct?" "Nononono!"

**Media:** The media player plays WMA, WMV, MP3 and MIDI files. You can set the ringtone to MP3/MIDI files. The quality of WMV files is quite excellent on the small screen (I took a 10MB Daily Show segment from crooksandliars and played it on there). I also ripped 2 Simpsons episodes from DVD to MPEG, then used Windows Media Encoder to convert to WMV (WME has a special compression profile for PocketPC devices). Each 30 minute episode at 320x240 ended up being about 40MB. However, the phone only has about 35MB onboard flash free (48MB flash onboard, the rest is used by the applications), so I can't see how a full episode looks. I'll be ordering a [2GB MiniSD card for $80](http://www.supermediastore.com/adata-2gb-minisd-card.html). I have a bet with a coworker about the battery life. I bet I could watch a full 2-hour movie on it before the batteries died, he doesn't think so.

**Browsers:** Pocket IE is crap. [Minimo](http://www.mozilla.org/projects/minimo/) is crap. Opera 8.5 beta for PocketPC is decent (particularly its ability to make regular webpages look decent on a small screen), but it is lacking in features. Also, there is an annoying bug. Before I say, let me remind you how PocketPC works: When you open an application, it loads into memory. When you close the window, the application remains in memory so it can be loaded more quickly the next time you want it. This way of doing things is also neat because it allows programs like IM clients to remain in the background, and pop themselves up if a message is received. (There are also protections in place so if you run out of memory (in my case, 48MB), the least used application is removed from memory first.) That being said, the Opera bug is that if it is in background memory, it'll pop itself up whenever you do nearly anything: turn on the phone's LCD, switch screen orientation, etc. From forum postings, this seems specific to Opera on the 8125 (HTC Wizard). The workaround is to do a force quit when I'm done (Ctrl-Q). Annoying, but hey, it's a beta.

**Skype:** I tried out the Skype client for 312mhz PocketPC ARM, and was not impressed. Half the time, loading the app would lock up the phone (see below), the interface wasn't that good, talking over GSM was impossible (occasionally, I would hear something that sounded like voice), and quality over ethernet/802.11 was blah. I didn't really use Skype anyways, so I ditched the app. 

**802.11:** Speaking of which, the phone includes 802.11b/g, so I could go to coffee shops and decrease my latency while chatting on IRC. The phone includes WEP and WPA-TKIP, but not WPA-AES. And on top of that, WPA-TKIP does not seem to work on my laptop at home using wpa_supplicant. It sucks that there is no AES support on the phone, but TKIP should work with my laptop configs (but doesn't), so I'm going along that route to see if I can figure out WHY it doesn't work. One day I may be able to switch my home's AP over to WPA-TKIP and use both my laptop and my phone over 802.11g at the same time.

**Crashing:** [pdx6](http://www.livejournal.com/users/pdx6){.lj-user} keeps asking "have you rebooted today yet?" With few exceptions, despite this being Windows, it has been very stable. The only times I've had to reboot have been when I was trying Skype, but Skype was crap anyways. The only other instability has been, occasionally when a new email arrived, and I clicked on it, it would go into the "please wait" animation and just sit there, and I couldn't click on anything within the app. However, I could close out of Outlook, go back in, and the animation would still be there against a blank background. HOWEVER, I could click on the area where the email item would be, and the email would load. When this happens (about 3 times in the last 5 days), all I had to do was go into the task manager and tell Outlook to close itself, and it would work fine the next time I opened Outlook. I've heard that this will be fixed with the next Wizard update.
