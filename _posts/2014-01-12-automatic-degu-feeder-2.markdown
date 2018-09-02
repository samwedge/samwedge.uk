---
layout: post
title:  "Automatic Degu Feeder 2"
categories: Arduino Pets
cover:  "/assets/posts/2014/degu_feeder2-6.jpg"
---


After making [Version 1][original-degu-feeder] of the Automatic Degu Feeder, I wanted to improve things by making it a little more stable and doubling the number of feedings. Rather than a cardboard tube, the latest version uses plastic pipe mounted onto MDF.

The code is similar to that used in Version 1, but has the capacity to control an additional servo. I have removed the slow movement of the servos because the latest version is more stable and less likely to fall apart. After initial testing, I also noticed some food remaining in the pipes so I added in a shaking motion to ensure that it all dropped out.

The code to make all of this work is on [GitHub][github]{:target="_blank"}. Just upload it to your Arduino and connect the servos to Pins 10 and 11.

The code basically waits for a set time and then initiates the initial feeding. It then waits until the second feeding should be performed and dumps the second load of food.

Here are some photos and videos of it in action.

{% include imageGallery.html url="/assets/posts/2014/degu_feeder2-1.jpg" group="degu_feeder2" title="One of the screws holding the tubes in place" %}
{% include imageGallery.html url="/assets/posts/2014/degu_feeder2-2.jpg" group="degu_feeder2" title="The servo is glued and screwed into place to keep it secure" %}
{% include imageGallery.html url="/assets/posts/2014/degu_feeder2-3.jpg" group="degu_feeder2" title="View of the top of the servo mounting" %}
{% include imageGallery.html url="/assets/posts/2014/degu_feeder2-4.jpg" group="degu_feeder2" title="The tube loaded and ready to feed the degus" %}
{% include imageGallery.html url="/assets/posts/2014/degu_feeder2-5.jpg" group="degu_feeder2" title="The Arduino is plugged into a USB adapter for mains power" %}
{% include imageGallery.html url="/assets/posts/2014/degu_feeder2-6.jpg" group="degu_feeder2" title="Degu feeder primed to drop the food into the cage" %}

{% include youtubePlayer.html id="kuV-eT0nds0" %}

[original-degu-feeder]: /posts/automatic-degu-feeder
[github]: https://github.com/samwedge/DeguFeeder