---
layout: post
title:  "Auto-Enabling NumLock on the Linux Mint Login Screen"
categories: Linux
cover:  "/assets/posts/2018/numlock1.jpg"
---

I use the number keys a lot. Not the line of numbers at the top of the keyboard, but the grid of numbers to the right. In order to use these numbers, you need to have NumLock set to on. This is just a simple keyboard press to activate it. However, if ever it is not already activated and I press the numbers, then the numbers act like arrow keys. It has always baffled me why these keys double as arrows, as there are arrow keys right next to them! But that's another complaint for another day. I like NumLock turned on and I make mistakes when it is off. So I would like it turned on by default.

Unfortunately, NumLock is off by default on Linux Mint. The is a menu option to turn it on, but as can be seen here, the menu option is deactivated by default.

System Settings -> Login indow -> Settings -> Activate Numlock

{% include imageGallery.html url="/assets/posts/2018/numlock2.png" group="numlock" title="The numlock option is deactivated" %}

To enable the menu option, we need to install `numlockx` first. Why there would be a menu option that uses a feature not installed by default is my second complaint, which again, I will leave for another day :)

{% highlight bash %}
sudo apt-get install numlockx
{% endhighlight %}

Then you should see that the menu item is enabled, so activate NumLock on startup.

{% include imageGallery.html url="/assets/posts/2018/numlock3.png" group="numlock" title="The numlock option is enabled after installing numlockx" %}
{% include imageGallery.html url="/assets/posts/2018/numlock4.png" group="numlock" title="The numlock option is now enabled by default" %}

Then sit back and relax as you can confidently type numbers without the fear that you will accidentally move the cursor by mistake...