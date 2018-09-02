---
layout: post
title:  "Rapiduino"
categories: Python Arduino
cover:  "/assets/posts/2017/rapiduino.jpg"
---

I’ve been working on a rapid-prototype interface between Python and an Arduino, which I have called Rapiduino. It still needs some work, but a fully-functional prototype has been completed and added to [GitHub][github]{:target="_blank"}.

Rapiduino is a Python library (fully tested in Python 2.7 and 3.4) to allow Python code to control an Arduino. The Python code runs on a computer connected to an Arduino through a serial connection.

First, you’ll need to upload a sketch to the Arduino. This is provided on GitHub with the Rapiduino library (tested on Uno and Mega). The Rapiduino library can then be used to connect to the Arduino and send it familiar commands such as digitalWrite and pinMode.

By sending these commands from Python and not writing them directly on the Arduino, you gain a few advantages:
* The power of Python’s wonderful syntax and libraries, allowing rapid development using a high-level language
* Easily integrate an Arduino with Python’s libraries to provide a real-time clock, web access, data visualisation, number crunching etc…
* Allow hot-swappable parts. Change pin mode, pin state etc. whenever you like from your Python code without having to recompile and upload a new sketch
* Easily obtain data from your Arduino without setting up any custom communication
* Deliver updates to your code over the internet from a remote location
* Probably many other benefits that will become realised as time goes on…
 
Of course, there are also some downsides. Don’t use this library if:
* You are not able to run a computer alongside an Arduino (not even a Raspberry Pi) because of issues such as size, battery, operating conditions etc.
* You need timing accuracy that Rapiduino does not yet support; For example, for an ultrasonic sensor where the connection lag could cause innacuracy
* Probably many others personal to your project…

Rapiduino is in active development. It is ready to be used in simple projects, but there may be a few issues if you are an early adopter. Please contact me or raise an issue on GitHub if you have any difficulties.

To install this package, you can use Pip:

{% highlight bash %}
pip install git+https://github.com/samwedge/rapiduino.git
{% endhighlight %}

Alternatively, copy the “rapiduino” package directory into your python site-packages or to a local directory within you project or python path.

It has been fully tested with Python 2.7 and Python 3.4.

To use with an ArduinoUno (for example), simply import the class and globals as follows

{% highlight python %}
from rapiduino.devices import ArduinoUno
from rapiduino.globals.arduino_uno import *
{% endhighlight %}

Set up the class and serial connection with the following. The port to be passed in can be identified using the Arduino software.

{% highlight python %}
arduino = ArduinoUno('/dev/ttyACM0')
{% endhighlight %}

Then start using it! Here is a blinking LED example:

{% highlight python %}
import time
while True:
  arduino.digital_write(13, HIGH)
  time.sleep(1)
  arduino.digital_write(13, LOW)
  time.sleep(1)
{% endhighlight %} 

You can also use classes for components (such as LEDs, Servos etc.) which make using the Arduino easier and less error-prone. The components are “bound” to the Arduino along with a pin-mapping which tells the Arduino object which pin is connected to which component pin. As an example, the LED class has one pin which can be initialised and connected to the arduino
as follows:

{% highlight python %}
from rapiduino.components.basic import LED
led = LED()
bindings = ((13, 0),)
arduino.bind_component(led, bindings)
{% endhighlight %} 

This creates an led object and binds it to the arduino. Binding it allows you to communicate with the led object, and let the led object talk to the arduino object. To bind an object, you need to specify bindings. This is basically a tuple of tuples to tell the arduino which pin is to be connected to each part of the component. In the case of the LED, there is only one pin to connect (Pin 0) which is connected to the arduino (Pin 13). Hence, the tuple looks like `((13, 0),)`

When binding, the code automatically takes care of checking compatibility, raising an error if there is a problem. For example, if you are trying to connect a component that requires a PWM pin to a non-PWM pin, you will get a helpful message.

Once the LED has been bound to the Arduino, you can re-write the blink example as:

{% highlight python %}
while True:
  led.toggle()
  time.sleep(1)
{% endhighlight %} 

 

The benefit of this is that you can use methods with familiar names such as:

{% highlight python %}
led.turn_on()
led.turn_off()
{% endhighlight %} 

You don’t need to think of pin numbers, pin states or pin modes beyond the initial set-up.

Please feel free to contribute code and/or suggestions on [GitHub][github]{:target="_blank"}.

[github]: https://github.com/samwedge/rapiduino