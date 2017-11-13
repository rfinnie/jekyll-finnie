---
date: 2017-08-19 13:51:17-07:00
excerpt: Wherein Ryan takes three days of security camera burst shot images, averages
  them together into a video and sets it to upbeat music.  You are being watched.
excerpt_standalone: true
layout: post
title: Big Brother in Motion
---
<div class="video-embed-lg">
<div class="video-embed video-embed-169">
<iframe width="640" height="360" src="https://www.youtube.com/embed/UD9sOe5eVzk" frameborder="0" allowfullscreen></iframe>
</div>
</div>

A few weeks ago, I installed a networked security camera overlooking the front yard.
It performs three functions:

1. It continually records a 640x480 10fps low-bitrate stream.
1. When motion is detected, it saves a 2304x1296 20fps high-bitrate recording of the event.  The camera can do 30fps, but only at 1920x1080 or lower, so lower framerate is an acceptable trade-off for a higher resolution.
1. When motion is detected, it saves a series of three 2304x1296 JPEG images, one second apart each.

The motion detection normally activates when a car drives by, so most of the burst shot images were of a car at three different positions.
I decided to average each of the sets together from a three day period, and the result was pleasing.

```
convert 00001a.jpg 00001b.jpg 00001c.jpg -evaluate-sequence mean 00001.jpg
```

From there, I converted the sets of averaged images into a video.

```
ffmpeg -framerate 5 -i %05d.jpg -c:v libx264 -profile:v high -crf 20 \
    -pix_fmt yuv420p -s 1920x1080 -r 30 output.mp4
```

The invocation converts the images into a 5fps video, forces yuv420p color (as the night shots were true greyscale), downscales to 1080p, and upconverts from 5fps to 30fps (as I wasn't sure if YouTube would properly handle a 5fps video).
The music was added post-upload from the YouTube audio library, and I'm very pleased with how well that track syncs up with the events in the video itself.

The input images were actually hand-curated a bit before making the video.
I removed all images of me (walking to the mailbox, backing out of the driveway, etc) and all images where it's unknown why the camera motion detection occurred (which are rare).

A large number of removed images were due to the separate security light underneath the camera, which has its own motion detection and is more sensitive and prone to false trigger, especially around dusk.
The security light will turn on, causing a camera event.
The camera will then realize it has enough light to switch from greyscale to color, causing another event.
A few minutes later, the light will turn off; third event.
The camera will switch from color to greyscale; fourth event.
Repeat a few times until both the light and camera settle on "really, it's dark now".

Also of note is the first day was a rare day (for Northern Nevada) when it rained.
A remote friend pointed out the craziness of it raining three times during a single afternoon, completely wetting the roads each time, and then completely drying out each time before the next rain.
Yeah, that happens.



