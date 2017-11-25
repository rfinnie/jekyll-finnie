---
date: 2017-11-24 18:19:05-08:00
excerpt: Wherein Ryan adds unnecessary complexity to his life, in the name of simplicity
excerpt_standalone: true
headline_image: /blog-media/2017/temperature-graph.png
layout: post
tags:
- homesweethome
title: 'Home Sweet Home: The "Things"'
---
It all began a year ago with a dot.

Late last year, when [replacing all my home's lights with LEDs](https://www.finnie.org/2017/02/05/home-sweet-home-light-bulbs/) (among other projects), I had been spending a lot of time at Home Depot and Lowes.  While at Lowes in November 2016, I noticed they sold Amazon Echo products, and the Echo Dot was only $50.  I picked one up and started playing with it.

Echo mostly works as you'd expect from a Siri-like device: you say "Alexa, ..." and it responds.  The full-sized Echo supposedly has decent sound, but the Dot is compact and has, at best, satisfactory sound for music.  However, if you want you may hook it up to a speaker via Bluetooth, and that works well.  I'll usually wake up with "Alexa, play some music", and it often does a good job figuring out music I would like to hear.  "Alexa, what's the weather like?"  "Alexa, what movies are playing?"  Etc.  A little limited in day to day functionality, but worth the $50 as a novelty.

But I was drawn to the developer functionality.  I wrote a few apps, one of which I tried to get submitted for publication, but they kept coming back with technical rejections to fix, and I never completed the process.  But it's still an interesting platform.

I was also curious about the "smart home" integration possibilities.  I had mostly dismissed the the whole "Internet of Things" idea as gimmicky, but there was one specific use case I could think of for me personally.  The backyard patio lights are a string of overhead lights which run to the side of the house and attach to a standard outdoor outlet which is in a slightly inconvenient location.  I had been thinking of ways to fix this, and a weatherproof remote controlled relay switch would be useful.

<img src="{{ "/blog-media/2017/smartthings-screenshot.png" | prepend: site.baseurl | prepend: site.url }}" alt="SmartThings screenshot" class="img-responsive img-rounded img-md pull-right">
The Samsung SmartThings hub was on sale on Black Friday 2016 for $50 (which as of 2017 is now its regular price), got good reviews, and seemed to have the best variety of device support.  Its two main supported protocols are ZigBee and Z-Wave.  Both are point-to-point mesh networks: you can pair both Device A and Device B to your hub, and if, say, Device B is not within range of the hub but can talk to Device A, Device A will relay the commands for it.  It also supports several other Internet-connected services, such as the thermostat I already happened to have.

I bought the SmartThings hub, as well as a weatherproof control module.  The pairing process was simple: press a button on the module to initiate pairing, accept it on the smartphone app, and now I could turn the patio lights on and off from my phone.  It also has Alexa integration, so once those were paired, I could say "Alexa, turn on patio lights".

A week later, I saw that Lowes had their Iris ZigBee (indoor) outlet control modules on sale, and bought a few to play with.  Iris is a competing home automation platform to Samsung SmartThings, but as I understood it, SmartThings supported Z-Wave and ZigBee devices from any manufacturer (which gave it a leg up on competitors which tend to be walled gardens).  I began the pairing process and... problem.  It simply showed up as "Thing" and I couldn't do anything with it.

I was all set to return them, but it was then I learned about the full extent of SmartThings compatibility.  While the app has a very simple interface and limited information and actions available, it turns out there is actually a web interface for developers.  Not only does this interface give you a lot more information (radio levels, debugging events, etc), but it actually allows you to [write your own device handlers](https://github.com/SmartThingsCommunity/SmartThingsPublic/blob/master/devicetypes/smartthings/zwave-switch.src/zwave-switch.groovy), in a scripting language called Groovy, which is basically the Java equivalent of Lua, and is often used in Jenkins plugins.

With enough work, I could have reverse engineered the Iris outlet and written my own device handler, but SmartThings has a large developer community, and it's likely *someone* has already written a device handler for nearly every Z-Wave and ZigBee device out there, even if it's not part of the core SmartThings platform.  I quickly found an Iris outlet handler, imported it, and then was able to pair the outlets.

An interesting feature of these Iris outlets is, while they are controlled via ZigBee, they also have a Z-Wave radio in them and can act as Z-Wave repeaters, strengthening the Z-Wave mesh as a whole.  However, SmartThings does not know about Z-Wave devices which literally do nothing but act as a repeater, so when I added the Z-Wave side, it would fall back to a switched outlet.  Not really a problem (in the phone UI, it would show a switch to toggle which did nothing), but it did inspire me to learn Groovy and how to write device handlers, and I wrote [a proper device handler for Z-Wave repeaters](https://github.com/rfinnie/smartthings/tree/master/devicetypes/rfinnie/zwave-repeater.src).  It was actually a surprising amount of work to write a handler for a device which does literally nothing.

<img src="{{ "/blog-media/2017/temperature-graph.png" | prepend: site.baseurl | prepend: site.url }}" alt="Temperature graph" class="img-responsive img-rounded img-lg">

Since then, I went a little overboard in the novelty of it, and have bought a number of additional devices.  As of now, the current list is:

* [Honeywell Wi-Fi thermostat](https://www.amazon.com/Honeywell-TH6320WF1005-Wi-Fi-Focus-Thermostat/dp/B00B0W070K) - Previously mentioned; I happened to have this for a few years before I bought the SmartThings hub.  It integrates with Alexa, but also with SmartThings, so I have it paired with SmartThings, which exports everything to Alexa and gives me more options than Alexa directly.  But honestly, it's rare I even touch the thermostat beyond its normal schedule.
* [GE Z-Wave outdoor module](https://www.amazon.com/GE-Wireless-Lighting-Control-12720/dp/B0013V8K3O) - Previously mentioned, currently serving the backyard patio lights.  They turn on each day at sundown, and off at 11:30PM.  (SmartThings supports dynamic "sunrise" and "sundown" in addition to static times.)
* [Leviton Z-Wave appliance module](http://home.leviton.com/products/z-wave-universal-plug-in-appliance-module/) - Currently serving some LED strip lights I have mounted in the alcove above the TV in the living room; I have them listed as "movie lights".
* [Iris ZigBee (Z-Wave) smart plug](https://www.lowes.com/pd/Iris-120-Volt-White-Smart-Plug/999925330) - Previously mentioned; I have two of these but they're not controlling anything at the moment.
* [Iris ZigBee smart fob](https://www.lowes.com/pd/Iris-Next-Gen-White-Wireless-Home-Automation-Fob/999925322) - A little fob with four buttons.  I can't remember what I've set them to do as I never use it.
* [GE Z-wave in-wall light dimmer](https://www.amazon.com/New-Model-Wireless-Lighting-Wall/dp/B01MUCZA1C) and [GE Z-Wave in-wall light switch](https://www.amazon.com/GE-Wireless-Lighting-Control-14291/dp/B01M1AHC3R) - Z-Wave switches which are meant to replace traditional in-wall light switches.  They work like normal light switches (and in the dimmer's case you can hold on/off to increase/decrease the light level), but are also Z-Wave controllable.  I have the dimmer on the front porch light and the normal switch on the outdoor rear wall light.
* [Cree ZigBee 60w equivalent LED bulbs](http://creebulb.com/connected-60-watt-replacement-soft-white) - Yes, the light bulb itself is ZigBee compatible.  I have four of them, two for the living room lights and two for the bedroom night stand lights.  I've got each of the pairs in device groups, so each night, it's "Alexa, turn off living room lights".  As for the bedroom, I have it set to turn them on at 11PM, as I'll usually go to bed shortly after and read for awhile.  Then "Alexa, turn off bedroom lights".  They're even dimmable ("Alexa, set living room lights to 50%.")  These are the devices I use most often day to day.
* [SmartThings ZigBee multipurpose sensor](https://www.amazon.com/Samsung-SmartThings-F-SS-MULT-001-F-MLT-US-2-Multipurpose/dp/B0118RQW3W) - This battery powered sensor has several measurements: ambient temperature, contact (like those small battery powered alarms you can add to windows), axis rotational position, and motion.  I currently have two, one mounted to the inside of the front door to monitor when it's opened, and one mounted to the garage door.  The device handler's "garage door" mode is interesting; rather than use the door contact sensor, it uses the axis rotational sensor.  When the sensor is vertical, the garage door is closed.  When it's horizontal, the garage door has been rolled up.
* [Zooz Z-Wave 4-in-1 sensor](https://www.amazon.com/ZOOZ-Z-Wave-Sensor-temperature-humidity/dp/B01AKSO80O) - Another sensor, though this provides some different measurements than the previous: temperature, humidity, light level and motion.  It even, oddly, has a tamper protection switch within the case which trips if opened, and is exposed as an acceleration event.  Mounted in the hallway, it's actually sensitive enough that you could figure out when I take a shower by graphing the humidity levels a few rooms over.
* [TP-Link Wi-Fi smart plug](https://www.amazon.com/TP-Link-Required-Control-Anywhere-HS100/dp/B0178IC734) - This is actually neither Z-Wave nor ZigBee, but straight Wi-Fi.  Normally it calls home and uses a separate app for control ("Kasa"), but it has a local port open, and accepts obfuscated commands (people, XOR is not encryption).  I wrote a [device handler and proxy](https://github.com/rfinnie/smartthings/tree/master/devicetypes/rfinnie/tplink-hs100-lan-web-proxy.src) to allow SmartThings direct control of it, and it currently controls a floor fan in the living room during the summer.