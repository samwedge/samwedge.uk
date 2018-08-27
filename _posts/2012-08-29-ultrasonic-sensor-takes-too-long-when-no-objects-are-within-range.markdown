---
layout: post
title:  "Ultrasonic Sensor takes too long when no objects are within range"
categories: Arduino
cover:  "/assets/posts/2012/ultrasonic_sensor1.jpg"
---


> ***Note (29 Nov 2015):*** *This article discusses the Ultrasonic library from “[Let’s Make Robots][lets-make-robots]{:target="_blank"}”. I have since found the [NewPing library][newping]{:target="_blank"} to be more up-to-date, customisable and useful. It does not suffer from the 1-second lag issue.*

I bought an ultrasonic distance sensor (HC-SR04) from eBay and was very impressed with how well it worked. I downloaded an Ultrasonic Library from [Let’s Make Robots][lets-make-robots]{:target="_blank"} which worked just fine. So well, in fact, that I bought another one.

When the second sensor arrived, I connected it to the Arduino. I was disappointed to notice that the sensor performed poorly when an object was not in range. My first sensor seemed to report a value of around 3500 cm when an object was not in range, whereas the second sensor returned a value of zero after a one second delay. It was more the delay than the value that was annoying – enough for my robot to crash into an obstacle between measurements.

Looking at the Ultrasonic library, I realised that there must have been something peculiar about my first sensor (maybe it had a built-in timeout on the ECHO pin?), as a value of zero after a one-second delay was exactly what it should have done if an object was outside range. The delay turned out to be easy to fix.

The Ultrasonic.cpp library file uses the function `pulseIn`, but only passes in two parameters. This function records how long a pin stays either `HIGH` or `LOW`, and takes the pin number and HIGH/LOW as arguments. The ECHO pin of the sensor goes `HIGH` after firing the sound, and `LOW` when it detects a returning signal. By measuring the time that the ECHO pin stays `HIGH`, it is possible to measure the distance to an object. If the ECHO pin stays `HIGH` forever (for instance, if the sound is lost by firing the sensor into the sky) then the `pulseIn` function has a timeout. The default value is one second. If the ECHO pin stays high for one second then a value of zero is returned.

It is possible to change the default by passing a third argument into the pulseIn function. The third argument states the timeout in microseconds. I have found that a value of 50000 microseconds works well for me.

Change:
{% highlight c++ %}duration = pulseIn(Echo_pin,HIGH);{% endhighlight %}

to:
{% highlight c++ %}duration = pulseIn(Echo_pin,HIGH,50000);{% endhighlight %}


{% include imageGallery.html url="/assets/posts/2012/ultrasonic_sensor1.jpg" group="ultrasonic_sensor" title="Ultrasonic Sensor" %}
{% include imageGallery.html url="/assets/posts/2012/ultrasonic_sensor2.jpg" group="ultrasonic_sensor" title="Ultrasonic Sensor" %}

[lets-make-robots]: https://www.robotshop.com/letsmakerobots/hc-sr04-ultrasonic-sensor
[newping]: https://playground.arduino.cc/Code/NewPing