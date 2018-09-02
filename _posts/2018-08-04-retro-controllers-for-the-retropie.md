---
date: 2018-08-04 23:52:52-07:00
excerpt: Wherein Ryan discusses retro (and not-so-retro) game controller options for
  retro games.  Retro.
excerpt_standalone: true
headline_image: /blog-media/2018/retropie-sn30-pro.jpg
layout: post
title: Retro controllers for the RetroPie
---
<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2018/retropie-sn30-pro.jpg" alt="RetroPie computer with 8Bitdo SN30 Pro" class="img-responsive img-rounded img-lg">

*This post was originally part of [a longer article about building a RetroPie game system](https://www.finnie.org/2018/09/01/building-a-retropie-game-console/).*

*Also, let me stress that controller design is a very personal preference, and has been known to cause intense discussions and the occasional bar fight.  The following are my own opinions.  Put down your pitchforks.*

My favorite controller is the Xbox 360 controller.  I consider its feel to be perfect, with one notable exception which I'll get into soon.  The analog sticks feel great, and are in the perfect locations.  The face buttons have just the right amount of travel.  The shoulder buttons are just long enough for my personal play style (using just the index finger to control both the trigger and shoulder buttons, with the finger pad controlling the trigger and the fleshy part near the finger base to bump the edge of the shoulder).

Many people prefer the Xbox One controller, but I think it's slightly inferior in many ways: the trigger and face buttons are a tiny bit too stiff, the length of the shoulder buttons are slightly shorter, and the analog sticks have a slight texture I don't prefer.  Don't get me wrong, overall I still rate the One controller as a very close #2, but still prefer the 360 controller.

Note that I've been avoiding talking about the D-pad, which was the exception I mentioned previously: the 360's D-pad is terrible.  I'm lucky if the direction I want is the direction I get.  The One's D-pad is light years better.  But it was rarely an issue in the 360 generation since the D-pad was often relegated to option selection (for example, cycling through weapons).

So why not simply use an Xbox controller on the RetroPie?  The Xbox controllers are what I call "analog-first"; that is to say, they work best when playing games designed for analog movement (so, modern).  In my hands, the most comfortable position is for the thumbs to be resting close to the rest of the hand.  On Xbox controllers, this means left thumb rests on the left analog stick, right thumb rests between the face buttons and the right analog stick.  Perfect for modern games.

But the RetroPie isn't a system for modern games.  In retro games, the D-pad is the main attraction.  I need a "digital-first" controller.  So what options are available for a RetroPie system?

## 8Bitdo SN30 Pro / SF30 Pro

<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2018/sn30-pro.png" alt="8Bitdo SN30 Pro" class="img-responsive img-rounded img-md pull-right">
The [SN30 Pro](http://www.8bitdo.com/sn30pro-sf30pro/) (or SF30 Pro; the only difference is Super Famicom-style cosmetics) is essentially a Bluetooth SNES controller, with the addition of dual analog sticks and trigger buttons.  Besides those additions, 8Bitdo has nailed it emulating the feel of the buttons compared to an authentic SNES controller.  If you grew up with the SNES, the SN30 Pro will feel exactly the same.

In my opinion, the Nintendo cross-style D-pad is the best for digital movement, so there are no compromises here.  And since the D-pad is the leftmost part of the SN30 Pro, it fits the "digital-first" desire.  But since it does include dual analog sticks and trigger buttons, it can be used for PS1 games or similar.

RetroPie bluetooth setup is relatively easy, once you put the SN30 Pro in Switch compatibility mode (hold Start+Y before pairing).  Input lag is present (as will be with any Bluetooth controller) but is negligible.

There are only a few downsides, one of which is the shoulder buttons are hard to locate while gaming.  See, the SN30 Pro is slightly thicker than an SNES controller, but is still rather thin.  Fitting both sets of shoulder and trigger buttons means the shoulder buttons are thinner than normal SNES shoulder buttons, and are harder to hit.  I've solved this by going into the libretro setup for each core which utilizes shoulder buttons only (SNES, GBA) and told it to treat the trigger button the same as the shoulder button.  Once I did that, I found myself subconsciously using the trigger buttons when needed, which work fine.

(For this reason, I would recommend against the SN30 Pro for modern gaming, especially with games like Mirror's Edge, which requires you quickly and accurately move between shoulder and trigger buttons.)

Another downside I've found is while the controller is supposed to sleep after 15 minutes of input inactivity, it doesn't appear to do so.  I'm guessing something with the RetroPie menu system is continually polling the controller and keeping it awake.  The solution is to remember to manually turn off the controller when you're done (hold Start for about 5 seconds until the status light turns off).

And finally, the exterior closely matches an original SNES controller, for good and for bad.  That is, it's a little small for adult hands, and ergonomics have progressed since the 90s, mainly wing grips for better holding.  Playing for hours on end can cramp my hands a bit.

The [8BitDo SN30 Pro+](http://www.nintendolife.com/news/2018/06/8bitdo_reveals_huge_range_of_bluetooth_controllers_all_compatible_with_nintendo_switch) is due to be released during the 2018 holidays, and looks to solve the shoulder/trigger issues and adds wing grips while otherwise seemingly keeping all the other functional elements identical.  However, it strays just a tiny a bit from the SNES clone aesthetic versus the original SN30 / SN30 Pro.  I have high hopes, and will be buying one when it's released.

## Original SNES / SFC controller

The original SNES controller is widely considered to be the best controller of its day.  Ergonomic (for the period), responsive, well built, and well laid out.  The issue is it's digital-only, so you're limited in game choice for emulated systems like the PS1.

You will need a USB adapter for the SNES controller, but those are readily available online.  I've got a pair of 15 year old Lik-Sang (rest in peace) adapters, which oddly aren't recognized by EmulationStation, despite appearing as standard USB gamepads on everything else I've used them on over the years.  I haven't actually looked into why they're not being recognized yet, but these specific adapters haven't been made in literally decades and plenty of other modern equivalent models exist.

*Edit: Turns out the Lik-Sang Super SmartJoy consumes too much current for the Raspberry Pi.  Daisy chaining it through a powered USB hub works fine.*

## Playstation DualShock 4

<img src="{{ site.url }}{{ site.baseurl }}/blog-media/2018/ds4-red.png" alt="Red DualShock 4" class="img-responsive img-rounded img-md pull-right">
Later I ordered a [Mayflash Magic-NS](https://www.amazon.com/Magic-NS-Wireless-Xbox360-Controller-Mayflash-Windows/dp/B07413R4HS) and a DualShock 4 controller.  The Magic-NS is a USB adapter which accepts nearly any mainstream wired or wireless controller (DualShock 3/4, Switch Pro, Xbox 360/One) and allows for play on the Switch or PC (using Dinput or Xinput).  Pairing and usage works fine, and I didn't perceive any additional input lag.

I'm not sure about the DualShock 4.  It fits my "digital-first" layout since the D-pad is on the top left.  I think the Nintendo-style D-pad is much better, but the Sony-style D-pad isn't bad.  It's more comfortable for long play than the SN30 Pro due to the modern wing grips.  And it works well for nearly all retro games, with one major exception: Super Metroid.

First, the option buttons (share/options) are flush/recessed with the controller, making them harder to press unless you really mean it.  This is great for most games since their retro equivalents (select/start) are rarely used during gameplay.  But by default, Super Metroid uses the select button heavily for weapon switching.  (Yes, I know you can remap buttons within the game, but I've got over 20 years of muscle memory built up.)

Also -- and this is something I didn't even notice until trying to play Super Metroid on the DualShock 4 -- the SNES's (and SN30 Pro's) face buttons are not equidistant.  The space between the top and bottom buttons are less than the space between the left and right buttons.  The result of this is it's much easier to quickly switch between the top, bottom and right buttons.  Super Metroid takes advantage of this, with the bottom being run, right being jump and top being fire.  (The game designers were likely well aware of this since they relegated weapon cancel to the left button, which is harder to hit if you're centering your thumb around the other three.)

I busted out the digital calipers and measured the distances between horizontal and vertical face buttons on a number of controllers:

* SNES / SN30: 17mm horizontal, 11mm vertical
* Switch Pro: 13mm horizontal, 11mm vertical
* Xbox One: 11mm horizontal / vertical
* Xbox 360 / DualShock 4: 13mm horizontal / vertical
* DualShock 3: 15mm horizontal, 13mm vertical

This explains why I was having a harder time with Super Metroid on the DualShock 4: it's harder to quickly move between three face buttons due to the equidistant layout and longer space between buttons.  Comfortable for two buttons at a time, but not three.

Super Metroid is a relative outlier here, since so many games only tend to rely on two face buttons at once, but considering it's one of my top five games of all time, it's a definite concern.

## Other controllers

As mentioned, the Magic-NS can adapt nearly any first-party controller, so there are no limits to personal preference.  If you prefer the D-pad to be on the bottom-left even for retro games, you could use an Xbox 360, Xbox One or Switch Pro controller.  I've got a DualShock 3, which has easier to press control buttons than the DualShock 4, but I don't like the feel of the wing grips on older DualShocks.  (Also, I'm pretty sure it's a counterfeit, but it's hard to tell since apparently Sony really lowered the design quality on the later-model DualShock 3s.)

And of course nearly any third-party wired controller should work fine.  I've got a horribly cheap feeling Logitech F310 controller.  I don't even remember buying it, but it somehow showed up in my box of USB accessories.  I would never consider using it for actual gameplay, but RetroPie supports it fine.
