---
layout: post
title:  "Schrödinger’s Cat Ice Bucket Challenge"
categories: 
  - Arduino
  - In the shed
  - Electronics
cover:  "/assets/posts/2014/ice-bucket-gareth.jpg"
---


The Ice Bucket Challenge has recently developed on social media as a “challenge” that you undertake to raise awareness for motor neurone disease. The idea is that you chuck a load of cold water over your head and donate to charity (or chuck a load of water over your head to avoid donating to charity). The rules vary since this has evolved through social media, with different people putting their own take on it.

I like the idea of charities and what they do. Whenever a friend does a sponsored event, I will donate to the cause. Inevitably, when people do sponsored events, they will choose a charity that has touched them personally. Therefore, when I sponsor a friend, I am donating to charities that help fight cancer, Parkinson’s, Alzheimer’s or motor neurone disease.

In order to give other charities a fair shout, when I am not sponsoring a friend, my charity money goes to those that make a difference behind the scenes. I personally donate to the [Royal Society for the Protection of Birds (RSPB)][rspb]{:target="_blank"}, the [British Trust for Ornithology (BTO)][bto]{:target="_blank"} and the [Invertibrate Conservation Trust (Buglife)][buglife]{:target="_blank"}.

The Ice Bucket Challenge has been made more interesting by inventive ideas such as that by [Bill Gates][bill-gates-youtube]{:target="_blank"}. However, convinced that he should have used a computer to make it more technical, I decided to enlist the help of my friend Gareth and make a microcontroller release mechanism for the buckets.

I am also a physicist, and relish any excuse to get a bit of science in there! This “challenge” is based on the Schrödinger’s cat experiment. This is a “thought-experiment” (ie. it has never actually been carried out in reality) devised by Erwin Schrödinger to point out the absurdity of the Copenhagen interpretation of quantum mechanics.

In the quantum world, where individual atoms and sub-atomic particles are studied, classical physics breaks down. Rather than being governed by equations that give an exact answer, the state of a particle at the quantum level is based on randomness and probabilities. In the Copenhagen interpretation of quantum mechanics, an individual atom’s state (such as whether a radioactive atom’s nucleus has decayed or not) is random and described by a probability. To find out what it is, we have to measure it. However, prior to being measured, it is said to be in both states simultaneously. By measuring it, we cause its superposition of states to collapse into one.

This doesn’t affect our everyday observations, because when we look at a pile of radioactive material we are looking at billions of atoms. With so many atoms, probabilities are extremely predictable and hence classical physics works.

What Schrödinger said, was if a small number of radioactive particles were placed near a detector set to release a hammer that will smash a vial of poison to kill a cat, then by checking if the cat was alive we have effectively measured the state of the radioactive particles. The quantum world has, in this instance, influenced the macroscopic world. If all of this was placed in a box so that we could not see the cat, then the particles are in a superposition of decayed and not-decayed states. Since the particles have both decayed and not decayed until measured, is the cat both alive and dead simultaneously?

{% include imageGallery.html url="/assets/posts/2014/ice-bucket-gareth.jpg" group="ice_bucket" title="Schrödinger’s Cat Ice Bucket Challenge" %}

Taking this into the realm of ice buckets, I used an Arduino microcontroller to generate a series of random numbers which are used to determine when each bucket will be released onto two cool cats (ie. Gareth and me). The buckets are balanced on a frame and each held in place by a servo motor. When the Arduino determines that the bucket should be released, the servo is rotated and the line holding the bucket is dropped.

In true Schrödinger style, we are simultaneously drenched and dry until you have watched this video!

The code used to produce this is fairly simple. I’ve put it on [GitHub][github]{:target="_blank"} for anyone interested.

{% include youtubePlayer.html id="08fOA6lGwIk" %}

[bill-gates-youtube]: https://www.youtube.com/watch?v=XS6ysDFTbLU
[rspb]: http://www.rspb.org.uk/
[bto]: https://www.bto.org/
[buglife]: https://www.buglife.org.uk/
[github]: https://github.com/samwedge/schrodingers-cat-ice-bucket-challenge
