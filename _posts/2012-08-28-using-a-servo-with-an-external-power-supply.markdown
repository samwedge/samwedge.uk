---
layout: post
title:  "Using a servo with an external power supply"
categories: Arduino Electronics
cover:  "/assets/posts/2012/external_supply.jpg"
---


I tried out the [Servo Sweep][servo-sweep]{:target="_blank"} example on the Arduino website but couldn’t get it to work properly. The servo would move a little and then stop, before juddering around a little more. I added in some code to write some information to the Serial monitor to help me to identify what was going wrong, but the serial monitor didn’t seem to work while the Servo was connected to the Arduino. By disconnecting the servo, it was clear that the code was perfectly fine and it was the servo at fault.

I noticed that the LED next to Pin 13 on the Arduino Uno (marked “L”) kept switching off, which led me to believe that the Arduino was resetting. It turned out that the Servo was pulling too much power from the Arduino causing it to reset every time it was instructed to move. A lot of examples on the Arduino website seem to suggest that a single servo can happily work while directly connected to the Arduino, and I have managed to _sort of_ get it to work every now and again, but I think it is pushing the capabilities of the power supply a little too hard, as I don't think it can provide that much instantaneous power to the servo _and_ power the Arduino at the same time.  

To remedy this, I resorted to an external power supply for the Servo. I used a bank of four batteries loaded into a battery holder. A quick test with the voltmeter indicated these were giving out around 5.5v. My servo is rated up to 6v, so good to go. I connected the red (positive) and brown (ground) wire of the servo to the external battery pack. The orange (signal) wire went to one of the PWM pins on the Arduino.

Since the signal current originated from the Arduino, it has to return to the Arduino. Therefore the ground wire from the external battery supply must be connected to the ground pin on the Arduino.

Once set up like this, the Servo worked beautifully.

{% include imageGallery.html url="/assets/posts/2012/external_supply.jpg" group="external_supply" title="Servo connected to Arduino with external power supply" %}

[servo-sweep]: https://www.arduino.cc/en/Tutorial/Sweep