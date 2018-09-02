---
layout: post
title:  "Installing the Wavelet Sharpen Plug-in for GIMP on Linux Mint"
categories: Linux
cover:  "/assets/posts/2015/wavelet_sharpen.jpg"
---


I wanted to install the wavelet sharpen plug-in for GIMP on Linux Mint, which you can get from the GIMP website. However, I found that the instructions for installing didn’t work on Linux Mint (and according to the GIMP website, also Ubuntu and Fedora). The comments on the GIMP website had the solution, but I didn’t find it obvious and there was a missing “$” symbol in the solution which may cause this solution to fail for others. Here is what I did to get it to work.

### 1
Open up a terminal as we’ll need to enter some commands to install.
### 2
You will need to install the libgimp2.0-dev package to compile GIMP plug-ins. In Linux Mint or Ubuntu, you can use the following:
{% highlight bash %}sudo apt-get install libgimp2.0-dev{% endhighlight %}

Fedora users will have to use a different command as you will have a different package manager. If you are unsure, open up a graphical package manager and search for `libgimp-2.0-dev` to install
    
### 3
Make a temporary directory to muck around in and change directory into it

{% highlight bash %}
mkdir ~/wavelet_temp
cd ~/wavelet_temp
{% endhighlight %}

### 4
Download the wavelet plug-in. You can do this by browsing to the plug-in website and downloading to the wavelet_temp directory that you have just created, or you can use the following command (assuming you want version 0.1.2, which was current at the time of writing):

{% highlight bash %}wget http://registry.gimp.org/files/wavelet-sharpen-0.1.2.tar.gz{% endhighlight %}

### 5
Now that we have downloaded it, we need to unzip it and extract the files from the archive
{% highlight bash %}
gunzip wavelet-sharpen-0.1.2.tar.gz
tar xvf gunzip wavelet-sharpen-0.1.2.tar
{% endhighlight %}

### 6
Now we can browse to the wavelet directory that you have just extracted:

{% highlight bash %}
cd wavelet-sharpen-0.1.2
{% endhighlight %}

### 7
From here, the instructions say to make and install. However, **this did not work for me** as I got an error after using make
{% highlight bash %}
# THIS DID NOT WORK FOR ME, SO DON'T BOTHER...
sudo make
sudo make install
{% endhighlight %}

Instead, open up the Makefile:

{% highlight bash %}
gedit src/Makefile
{% endhighlight %}

and change line 16 from

`$(CC) $(CFLAGS) -o $@ $^ $(LIBS)`

to

`$(CC) $(CFLAGS) -o $@ $^ -lm $(LIBS)`

### 8
The install should now work, so type in:

{% highlight bash %}
sudo make
sudo make install
{% endhighlight %}

### 9
Now we have compiled the wavelet-sharpen plug-in. Check that it has the correct permissions (it should be executable). By typing in the following:

{% highlight bash %}
ls -l wavelet-sharpen
{% endhighlight %}

you should get something like the following, with the three “x” characters over on the left:

{% highlight bash %}
-rwxr-xr-x 1 root root 34730 Aug 16 11:52 src/wavelet-sharpen
{% endhighlight %}


### 10
We can now stick the plug-in into the GIMP plug-ins folder. I have GIMP 2.8 and used the following command:

{% highlight bash %}
cp src/wavelet-sharpen ~/.gimp-2.8/plug-ins/
{% endhighlight %}

Change the directory name to whatever version of GIMP you have. If you are unsure, you can go to GIMP and select “About” from the “Help” menu. For me, this is “2.8.10”, so the directory name is “.gimp-2.8”. Another way to do this is to type part of the above command and use auto-complete to tell you what directories are available. In other words, type the following:

{% highlight bash %}
cp src/wavelet-sharpen ~/.gimp-
{% endhighlight %}

but instead of hitting <enter>, hit the key <tab>. This will either auto-complete the directory name for you, or will show you what options are available (you may need to double-tap the <tab> key if it doesn't auto-complete straight away).

### 11
Once that is in the right place, we now need to clean up after ourselves by deleting the files that were used to compile the plug-in. Since everything was put into a temporary directory, we can just delete this.

{% highlight bash %}
cd ~
rm -r ~/wavelet_temp
{% endhighlight %}

### 12
Start up GIMP (or restart it if it was already open). You should now find the plug-in on the menubar under “Filters” –> “Enhance” –> “Wavelet-sharpen”.