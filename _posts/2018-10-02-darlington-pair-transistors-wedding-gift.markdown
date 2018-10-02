---
layout: post
title:  "Darlington Pair Transistors Wedding Gift"
categories: Electronics
cover:  "/assets/posts/2018/darlington_pair8.jpg"
---

I am terrible with presents. Every now and again I have an idea of what to get somebody for Christmas or for their birthday and inevitably I forget before the event. But when it came to the wedding of one of my friends, I had an very definite idea of what to make that would not leave my mind. His surname is Darlington. I reasoned that once he was married, he would form a Darlington Pair. In electronics, a [Darlington Pair][darlington_pair_wikipedia]{:target="_blank"} is made when one transistor feeds into another, creating a super sensitive switch that can be activated by touch. My wedding present would be a device which, when touched by two people, and those two people touch each other, an LED lights up. Of course I had to package it as a wedding gift. I couldn't just present them with a hastily soldered circuit board. But this is where my 3D printer comes in handy :)

I used FreeCAD to design the enclosure.

{% include imageGallery.html url="/assets/posts/2018/darlington_pair1.png" group="darlington_pair" title="FreeCAD" %}

And Fritzing to design the circuit layout prior to soldering. Even on a small circuit such as this one, I like to play around with the layout in Fritzing to make sure that I get the components in the right place and cut the tracks correctly.

{% include imageGallery.html url="/assets/posts/2018/darlington_pair2.png" group="darlington_pair" title="Schematic" %}
{% include imageGallery.html url="/assets/posts/2018/darlington_pair3.png" group="darlington_pair" title="Stripboard Layout" %}

I then printed the enclosure using my 3D printer. This took a couple of attempts and test prints to get right. I needed to test out how much tolerance to add in to the lip so that the lid could fit snugly onto the base. I also needed to test how large the diameter should be for the bolt hole so that the bolt can grip on without cracking the plastic. I did these test prints on small sections of the enclosure, rather than printing the whole thing and wasting time, plastic and money. To test the bolt hole diameter, I printed a set of cylinders mounted to a small base each with a slightly different tolerance (2.9 mm, 3.0 mm and 3.1 mm) to fit a 3 mm bolt. It turns out the 3.0 mm hole was perfect when printed with my Ultimaker. I did a similar small-scale test to ensure the lid lip was correct.

{% include imageGallery.html url="/assets/posts/2018/darlington_pair4.jpg" group="darlington_pair" title="3D printing the enclosure" %}
{% include imageGallery.html url="/assets/posts/2018/darlington_pair5.jpg" group="darlington_pair" title="3D printing the enclosure" %}

I then assembled all of the components together

{% include imageGallery.html url="/assets/posts/2018/darlington_pair6.jpg" group="darlington_pair" title="Inserting the electronics" %}
{% include imageGallery.html url="/assets/posts/2018/darlington_pair7.jpg" group="darlington_pair" title="Adding the battery" %}

The completed device was inscribed with a message to the happy couple. I decided to have the transistors poking through the top of the enclosure to symbolise their union (the two transistors working together). The two screws exposed on the surface, when touched together, will cause the LED to light up. If one person touches the left screw, and another touches the right screw, then the LED will only light up when the two people touch each other.

{% include imageGallery.html url="/assets/posts/2018/darlington_pair8.jpg" group="darlington_pair" title="Testing the finished product" %}

[darlington_pair_wikipedia]: https://en.wikipedia.org/wiki/Darlington_pair