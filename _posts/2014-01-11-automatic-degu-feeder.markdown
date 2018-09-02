---
layout: post
title:  "Automatic Degu Feeder"
categories: Arduino Pets
cover:  "/assets/posts/2014/degu_feeder2.jpg"
---


> *Update:* I have since updated this design with the [Automatic Degu Feeder Version 2][updated-degu-feeder]

Degus are sociable animals and crave attention, so they should not be left unattended for long periods of time. However, there are always occasions when you are not going to be around. Just such an occasion came by recently, so I needed to knock up a quick automated degu feeder to ensure that they are at least fed while I am away. I would rather not simply pour lots of food into their cage because they are likely to overfeed and so I needed something to maintain their current feeding pattern of a morning and an afternoon feeding.

Since I needed to knock this feeder up pretty quickly, I had to use what was available around me. This is the reason for the use of the cardboard toilet roll tube instead of something a bit more professional (just getting my excuses in early…).


The feeder is actually really simple, and is just a small servo connected to an Arduino. I screwed the servo onto a solid base (in my case, I used an old homemade degu toy, but any solid wooden base will do). I then bent a cardboard tube into a V shape and used sellotape to maintain the shape. I pierced a hole through the middle of the tube so that I could mount it onto the servo horn. A crocodile clip was then used to secure this in place.

The whole thing was a little fragile, so I could not use full speed when using the servo to tilt the tube. Instead, a small delay was added between a series of small positional movements to limit the speed.

The code to make all of this work is on [GitHub][github]{:target="_blank"}. Note that this code is for [Version 2][updated-degu-feeder] of the feeder, but it works the same if you just don't connect the second servo. Just upload it to your Arduino and connect the servo to Pin 10.

The code basically waits for a set time and then initiates the initial feeding. It then waits until the second feeding should be performed and dumps the second load of food.

Here are some photos and videos of it in action.

{% include imageGallery.html url="/assets/posts/2014/degu_feeder1.jpg" group="degu_feeder" title="Automatic Degu Feeder" %}
{% include imageGallery.html url="/assets/posts/2014/degu_feeder2.jpg" group="degu_feeder" title="Automatic Degu Feeder" %}

{% include youtubePlayer.html id="SmU2irDwP5w" %}

[updated-degu-feeder]: /posts/automatic-degu-feeder-2
[github]: https://github.com/samwedge/DeguFeeder