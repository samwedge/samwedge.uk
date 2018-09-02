---
layout: post
title:  "Website Image Grabber"
categories: Python
cover:  "/assets/posts/2014/website-image-grabber.jpg"
---


I wrote a simple Python script to grab images from a website at set intervals. It can be used to collect images from an online webcam so that you can put them together into a timelapse video, or simply to keep a record.

The code has a section labelled “USER SETTINGS”. In this section, you can define a list of websites under the “paths” variable. Each of these links will be checked by the code and the images downloaded. The “periodic” variable can be “True” or “False”. If True, an image will be captured every “t” seconds. Note that this doesn’t strictly download the images every t seconds because I have done this a lazy way. It is every t seconds + the time taken to download and process the images. To get around this, you can set “periodic” to False (which will instruct the code to run once only) and use another tool (such as [cron][cron]{:target="_blank"} or [Airflow][airflow]{:target="_blank"}) to run the code periodically.

When the code first runs, it will create a directory called “IMAGES”. In this directory, it will create a separate directory for each website in the “paths” list. Each time it downloads an image, it will perform a check to see if that image has already been downloaded. If so, it will discard the latest download. If not, then it will add it to the directory.

It requires Python to run.

The source code is on [GitHub][github]{:target="_blank"}.


[github]: https://github.com/samwedge/ImageGrabber
[cron]: https://en.wikipedia.org/wiki/Cron
[airflow]: https://airflow.apache.org/