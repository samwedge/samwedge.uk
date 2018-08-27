---
layout: post
title:  "Using Garmin eTrex Summit on Ubuntu Linux"
categories: Arduino
cover:  "/assets/posts/2013/ultrasonic_distance2.jpg"
---


I have a Garmin eTrex Summit that I wanted to connect to my laptop to extract location data. I had previously bought a cable to connect it to my computer via USB. This cable came with drivers for Microsoft Windows. However, I currently use Ubuntu Linux, so the drivers and software provided with the Garmin were useless to me.

I installed Gebabbel from the Ubuntu Software Centre. This is a user-interface that links in to the command-line GPSBabel software to communicate with a whole range of GPS devices.

I had trouble getting Gebabbel to talk to the Garmin. It turned out that this was because the eTrex Summit actually communicates over a Serial connection and not USB as I expected from the cable type. The eTrex Summit has an in-built USB to Serial converter, so the computer doesn't actually see the Garmin at the end of the USB cable - it sees a serial port instead. To get it to work, I had to double-click on the Garmin device that appeared in the Source list in GeBabbel and change the Path/Device from `USB` to `/dev/ttyUSB0`.

{% include imageGallery.html url="/assets/posts/2013/gebabbel1.png" group="gebabbel" title="GeBabbel user interface" %}
{% include imageGallery.html url="/assets/posts/2013/gebabbel2.png" group="gebabbel" title="GeBabbel source selection" %}

This assumes that your Garmin is on port `ttyUSB0`. You can find this out by opening a terminal window and typing `dmesg | grep tty`. This outputs changes to the serial port. Run the command just after inserting the USB cable, and you should see something like the following:

{% highlight markdown %}
username@computername:~$ dmesg | grep tty
[ 0.000000] console [tty0] enabled
[ 1201.769454] usb 5-1: pl2303 converter now attached to ttyUSB0
{% endhighlight %}

As you can see, the converter is attached to `ttyUSB0`. This should be in the `/dev` directory, so the full path is `/dev/ttyUSB0`

After inputting this address, GeBabbel communicated with the Garmin perfectly.
