---
layout: post
title:  "Ultrasonic Distance Meter"
categories: Arduino
cover:  "/assets/posts/2013/ultrasonic_distance2.jpg"
---


I have wanted to try making something using an Atmega 328p without having to purchase an Arduino specifically for the job. The Arduino Uno costs around £20 (the official Arduino, that is), so using one for a permanent project is costly. In contrast, an Atmega 328p can be bought for under £2, with associated circuitry being only around an extra £1 or so. If bought in bulk, then the price goes down. (*EDIT August 2018: Third-party "Arduino" Nanos can now be found sub-£2 if bought in bulk and without the hassle of soldering multiple components together*)

I followed a [tutorial][tutorial]{:target="_blank"} on the Arduino website that shows the basic circuit for an Atmega 328p. This can be programmed using an Arduino Uno. There are some methods on how to program the chip using an external programmer (at extra cost) or by removing the chip from the Arduino Uno itself and placing the 328p to be programmed in its place (which I imagine would eventually fatigue the Uno chip holder causing mechanical failure). However, I prefer the method on this [website][atmega-chip-programming]{:target="_blank"} that allows the Arduino to be used without pulling out the chip and risking damage.

For this project, I decided to make a device that displays the distance to an object. It uses an ultrasonic sensor (HC-SR04) to obtain the distance and three seven-segment displays side-by-side to display the result. The [NewPing library][newping]{:target="_blank"} is used to control the ultrasonic sensor.

It also features an alarm buzzer, which gives audible feedback on the proximity of an object to the sensor. The sensor has an alarm range defined. If the range is entered (let’s say the object is within 100 cm of the sensor) then the buzzer begins to beep periodically. As the sensor approaches an alarm distance (let’s say 10 cm) then the buzzer beep period gets smaller until eventually the buzzer plays a continuous tone. This is a bit like those sensors on the back of cars to help the driver park without crashing.

I decided to make use of one of the external interrupts on the Atmega chip. I linked it to a button that allows the user of the device to set the alarm distance. If you want to start the buzzer beeping at 150 cm, and set the alarm distance to be 50 cm, then move the sensor until the display reads out 150 cm and push the button. The indicator LED will flash showing that the distance is in the process of being set. Move the sensor so that the distance reads out 50 cm and press the button again. The indicator LED will now stay on to show that a custom setting has been recorded and is in use. To cancel and revert back to the hard-coded default values, just push the button and the LED will go off.

I had 5 pins spare on the Atmega chip after the above was all wired up. I decided to use these to display a scale showing the position of the sensor relative to the alarm distances. When crossing the first alarm distance, when the buzzer begins to beep, the first green LED will light up. As the sensor is moved closer to the object, the second green LED will light up, then the third and then the fourth. Once the alarm distance has been reached, the red LED will light up. By wiring these LEDs to the PWM pins on the Atmega chip, I can make each LED fade in or out based upon the distance to the object.

If you would like to make this, you will need:

* 4 x green LEDs
* 3 x red LEDs **
* 1 x LM7805 5v Linear Voltage Regulator *
* 1 x 9v PP3 Battery
* 1 x Diode *
* 1 x HC SR04 Ultrasonic Sensor
* Atmel Atmega 328P-PU Microcontroller *
* 1 x 5v Buzzer
* 2 x SPST Switches
* 2 x Push Switches (normally open) **
* 3 x 7-segment LEDs
* 2 x 22pf Ceramic Capacitors *
* 2 x 10uf Electrolytic Capacitors
* 1 x 16MHz Clock Crystal *
* 2 x 10k Ohm Resistors *
* 22 x 220 Ohm Resistors **
* 6 x 330 Ohm Resistors
* Wire
* Stripboard/breadboard
* Terminal Header *

If using an Arduino Uno, the items marked with a * are not needed, as they are also present on an Arduino Uno. The quantity of items marked ** can be decreased by 1, as there is one present on the Arduino Board.

There wasn’t very much thinking behind the value of the 220 and 330 ohm resistors. By all means, replace the 220 ohms with 330 ohms (or vice versa) if you have more available. It will make little difference to the circuit. Also, it will make no difference to this circuit if you replace the 21 resistors connected to the 7-segment display anodes with just 3 resistors connected to the common anodes, due to the way that the program operates them (by multiplexing). I only did it this way because I wanted to future-proof the circuit in case I wanted to change the program.

Below are some pictures of the sensor, along with the schematics if you wanted to build one yourself. I soldered the components onto stripboard and mounted onto some MDF. I used a glue gun to stick a battery onto the MDF – a bit crude, but I’m not looking to win any awards. This project is only an excuse for me to test out the Atmega chip and to give soldering onto stripboard a go.

For the seven segment display, I used the [Arduino Seven Segment library][sevseg-library]{:target="_blank"}. Unfortunately, this only works for a 4-digit display. I have used 3 individual 7-segment displays together to form a single 3-digit display (I only used three because the distance that I want to display only goes up to 500). I modified the code (SevSeg.cpp and SevSeg.h) to work for three segments. All files can be found on [my Github][github]{:target="_blank"}. Just paste it into text files in a subdirectory of your Arduino Libraries directory called SevSegThree.

{% include imageGallery.html url="/assets/posts/2013/ultrasonic_distance_schematic.svg" group="ultrasonic_distance" title="Ultrasonic Distance Meter Schematic" %}
{% include imageGallery.html url="/assets/posts/2013/ultrasonic_distance1.jpg" group="ultrasonic_distance" title="Ultrasonic Distance Meter" %}
{% include imageGallery.html url="/assets/posts/2013/ultrasonic_distance2.jpg" group="ultrasonic_distance" title="Ultrasonic Distance Meter" %}

{% include youtubePlayer.html id="HHVxj9JSdGo" %}

[tutorial]: https://www.arduino.cc/en/Main/Standalone
[atmega-chip-programming]: http://blog.biophysengr.net/2012/06/writing-sketches-to-atmega386p-pre.html
[newping]: http://playground.arduino.cc/Code/NewPing
[sevseg-library]: http://playground.arduino.cc/Main/SevenSegmentLibrary
[github]: https://github.com/samwedge/ultrasonic-distance-meter