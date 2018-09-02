---
layout: post
title:  "Configuring a Webserver Running Apache and Nginx"
categories: Linux
cover:  "/assets/posts/2017/nginx.jpg"
---

## Introduction
This post serves as a reminder to myself what the basic steps are to configure my webserver. Hopefully it will help others. There are some additional steps I took following those below, such as to configure Python, but that is outside the scope of this post.

I was prompted to put this together because I wanted to understand how everything bolts together. I was using a server running Plesk which configured everything for me, but whenever I wanted to do anything “not usual” such as configuring Python via WSGI, I found myself editing configuration files that stated something like “this file was auto-generated and will be overwritten”. As such, a lot of configuration was done through mouse-clicks on the Plesk web interface – great for beginners as it masks you from the details. I found myself running a webserver without knowing what was going on in the background. Not so great if things start to go wrong, or if you want to do something that is not available via Plesk. I am also a big believer in “infrastructure as code”. If I can automate processes then I will. Configuration via mouse-clicks is not easy to sustain or transfer to another webserver.

The steps below are the basic steps required to configure a webserver with multiple virtual hosts, running Nginx to handle requests and serve static content, and Apache to serve dynamic content.


## Assumptions
You have Ubuntu 16.04 installed
You aren’t afraid of the command line, ssh etc…
You have backed everything up – I will not take responsibility for lost files yadda yadda…..
You have full root access to your server
You know your server’s public IP address (denoted in this tutorial by x.x.x.x)


## General Overview
Apache is known to be resource-intensive, but great at generating dynamic files. Nginx is known to be nimble and great for static content, but not great for dynamic content. By combining the two, you can have the best of both worlds.

I’m oversimplifying, but the general path of a static request will look like this:

Client requests static page `foo.html` from the server on Port 80
Nginx grabs the file from disk and serves it to the client
For dynamic content, it looks like this:

Client requests dynamic page `foo.php` from the server on Port 80
Nginx connects to Apache on Port 8079 (for example)
Apache constructs the static content, returns it to Nginx and goes back to sleep
Nginx serves the content back to the client
The benefits are:

Apache, which is resource hungry, only has to deal with dynamic content
Apache does not have to deal with clients on slow connections and can release resources as soon as it has served the content to Nginx
Nginx deals with client on slow connections, freeing up Apache to deal with the things it is good at
If the webserver is busy and Apache has no free threads to deal with requests, Nginx can still serve a useful response to the client, such as a message to come back later. This is far better than the server simply hanging


## Let’s get started
#### Log in as root user through SSH
{% highlight bash %}
ssh root@x.x.x.x
{% endhighlight %} 


#### Create a new non-root user with sudo priviledges:
We don’t want to use the root account if we can help it. Let’s set up a user account with sudo privileges so we can do what we like, but with a degree of safety. Enter the following commands, but change username to the name of the user that you want:

{% highlight bash %}
adduser username
usermod -aG sudo username
{% endhighlight %} 


#### Switch to your newly-created account and test sudo works
{% highlight bash %}
su username
ls /root         #(should get permission denied error)
sudo ls /root    #(should give a list of directories)
{% endhighlight %} 


#### Ensure repositories and packages are up to date
{% highlight bash %}
sudo apt-get update
sudo apt-get upgrade
{% endhighlight %} 


#### Install some useful tools, if not already installed
`nano` is the text editor we will use. `man-db` allows you to view the manual pages of commands, for example by typing `man ls`

{% highlight bash %}
sudo apt-get install man-db nano
{% endhighlight %} 


#### Install Apache, if not already installed
This was already installed on my webserver, but if not you can install it with:
{% highlight bash %}
sudo apt-get install apache2
{% endhighlight %} 
 

#### Bind apache to port 8079 instead of default 80
Apache will be running on Port 80 by default. Instead, we want Nginx to be exposed on Port 80. Let’s change Apache to be on Port 8079 by editing one of Apache’s configuration files:

{% highlight bash %}
sudo nano /etc/apache2/ports.conf
{% endhighlight %} 

Change the line that says `Listen 80` to `Listen 8079`

Then we’ll need to change it in the Virtual Host file:

{% highlight bash %}
sudo nano /etc/apache2/sites-enabled/000-default.conf
{% endhighlight %} 

Change <VirtualHost *: 80> to <VirtualHost *: 8079>

Tell Apache to reload the config file:

{% highlight bash %}
sudo /etc/init.d/apache2 reload
{% endhighlight %} 

If you like, you can temporarily add port 8079 to the firewall whitelist to test this port loads the default Apache page. If all is ok, remove it from firewall as we will only ever talk to Apache internally via Nginx. My webhosting company gives me access to do this from a user-interface. You can then access the page via a specific port by browsing to `x.x.x.x:8079`


#### Set up Nginx

Install Nginx:

{% highlight bash %}
sudo apt-get install nginx
{% endhighlight %} 

You can start the server with:

{% highlight bash %}
sudo /etc/init.d/nginx start
{% endhighlight %} 

Check that Nginx is running ok by browsing to your IP address. Since Nginx looks for website files in the same directory as Apache, it may pick up Apache’s index.html and look like Apache is serving webpages, but remember that Apache is running from a different port so this is not the case. If you want to be absolutely sure, you can stop Apache and test that web pages are still being served by typing in:

{% highlight bash %}
sudo /etc/init.d/apache2 stop
{% endhighlight %} 

We are first going to configure Nginx to handle all requests to your server, whether directly to your IP address, or via domain names, in the same manner. We will come back to this later to change its behaviour so that if a user browses to your site via a domain name, we will serve up files specific to that domain. For now though, let’s just get the basics working.

There are two important directories located in /etc/nginx:
* sites-available
* sites-enabled

`Sites-enabled` contains configurations for each of the websites hosted on your server. But rather than keep the configuration files in directly in `sites-enabled`, it is good practice to keep them in `sites-available` and create a symbolic link (shortcut) in `sites-enabled`. This provides a place to keep a configuration file for each of your sites and allows you to enable/disable each site by create/deleting the symbolic link without touching the original configuration file.

There is a default configuration file in sites-enabled and sites-available. We will delete these, and create our own:

{% highlight bash %}
sudo rm /etc/nginx/sites-available/default
sudo rm /etc/nginx/sites-enabled/default
{% endhighlight %} 

We can then create our own file for all requests. I will call this file ip because it will eventually become the config file for all accesses to your server via the IP address (or where the host in the HTTP header can not be resolved). As said above, for now this will handle all requests via all domains that point to your server.

{% highlight bash %}
sudo nano /etc/nginx/sites-available/ip
{% endhighlight %} 

And add the content:

{% highlight nginx %}
server {
    listen 80 default_server;
 
    root /var/www/html/;
    index index.php index.html index.htm;
 
    server_name _;
 
    location / {
        try_files $uri $uri/ /index.php;
    }
 
    location ~ \.php$ {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header Host $host;
        proxy_pass http://127.0.0.1:8079;
    }
 
    location ~ /\.ht {
        deny all;
    }
}
{% endhighlight %} 

The important parts of this configuration file are:
* `listen 80 default_server;` tells Nginx to listen on Port 80 and use this config file for any hosts that can not otherwise be resolved
* `root /var/www/html;` Tells Nginx where to look for files
* `proxy_pass http://127.0.0.1:8079;` Tells Nginx to pass any requests for PHP files (dynamic content) through to Apache on Port 8079

We now need to create a symbolic link in `sites-enabled` and reload Nginx to enable the config file to be picked up:

{% highlight bash %}
sudo ln -s /etc/nginx/sites-available/ip /etc/nginx/sites-enabled/ip
sudo /etc/init.d/nginx reload
{% endhighlight %} 


#### Let’s Re-Cap
By now, we have performed the following:

Apache is running on Port 8079.
* Nginx is running on Port 80
* All static content is served by Nginx
* All dynamic content in PHP files is served by Apache, via Nginx
* Port 80 should be open and Port 8079 should be closed, forcing all requests through Nginx
* All incoming traffic to your webserver should be pointed at files located in `/var/www/html/`, whether that traffic is through a domain name or your server’s IP address
We will now look at separating out the traffic from different domain names, allowing you to host several sites on the same server. We will have to perform the following:

1. Create the relevant directories from which content will be served
2. Modify Apache configuration files, so that Apache knows where to go for dynamic content for each domain
3. Modify Nginx configuration files, so that Nginx knows where to go for static content for each domain


#### Creating Webspace Directories
We will now create three directories, one for traffic directly to the IP and one for each of two example domain names. Each webspace directory will contain a public folder where files that should be served can be stored. This will then give you a place to store other files in the future that you do not want to be served, such as configuration or password files.

{% highlight bash %}
cd /var/www
mkdir -p ip/public
mkdir -p example1.com/public
mkdir -p example2.com/public
sudo chmod -R 755 /var/www
{% endhighlight %} 

Change the permissions of these directories – we don’t want to use sudo every time we edit a file here. The following commands will change the owner to the currently logged in user.

{% highlight bash %}
sudo chown -R $USER:$USER /var/www/ip
sudo chown -R $USER:$USER /var/www/example1.com
sudo chown -R $USER:$USER /var/www/example2.com
{% endhighlight %} 

We can now also remove the html directory currently used by Apache/Nginx as we will no longer be needing it:

{% highlight bash %}
sudo rm -r /var/www/html
{% endhighlight %} 

Let’s tell Nginx that we have moved the default directory:

{% highlight bash %}
sudo nano /etc/nginx/sites-available/ip
{% endhighlight %} 

and change the line root /var/www/html/; to root /var/www/ip/public/;

Tell Nginx to reload the file:

{% highlight bash %}
sudo /etc/init.d/nginx reload
{% endhighlight %} 

Now let’s create an example static html file, and an example dynamic php file. This will allow us to test that each site works with both static and dynamic content:

{% highlight bash %}
echo "ip static" >> /var/www/ip/public/static_test.html
echo "example1.com static" >> /var/www/example1.com/public/static_test.html
echo "example2.com static" >> /var/www/example2.com/public/static_test.html
echo "<?php echo('ip dynamic'); ?>" >>ip/public/dynamic_test.php
echo "<?php echo('example1.com dynamic'); ?>" >>example1.com/public/dynamic_test.php
echo "<?php echo('example2.com dynamic'); ?>" >>example2.com/public/dynamic_test.php
{% endhighlight %} 


#### Setting up virtual hosts in Apache
Apache has the same `sites-available` and `sites-enabled` style of configuration as Nginx. Let’s change directory to the available sites:

cd /etc/apache2/sites-available/

If you list the files with `ls`, you should see a file called `000-default.conf`. This is the default config file for all requests to Apache. We need to point this to our newly-created webspace. Edit the file:

`sudo nano 000-default.conf`

and change the `DocumentRoot` to our new location: `DocumentRoot /var/www/ip/public/`

We can use this file as the basis for the config for our other domains:

{% highlight bash %}
sudo cp 000-default.conf example1.com.conf
{% endhighlight %} 

Edit the file and let’s make some changes:

{% highlight bash %}
sudo nano example1.com.conf
{% endhighlight %} 

* Set the ServerAdmin email address to something useful.
* Set the DocumentRoot to the correct location: DocumentRoot /var/www/example1.com/public/
* Add the line: ServerName example1.com with your main domain name for this webspace
* Add the line: ServerAlias www.example1.com with any aliases or subdomains that you want to point at this webserver

Repeat the above for your other domain names (example2.com in this tutorial).

Enable the virtual hosts by adding a symbolic link in sites-enabled:

{% highlight bash %}
sudo ln -s /etc/apache2/sites-available/example1.com.conf /etc/apache2/sites-enabled/example1.com.conf
sudo ln -s /etc/apache2/sites-available/example2.com.conf /etc/apache2/sites-enabled/example2.com.conf
{% endhighlight %} 

Restart apache and test:

{% highlight bash %}
sudo /etc/init.d/apache2 reload
{% endhighlight %} 

At this point, the static HTML pages should all still point to /var/www/ip/public/ because Nginx does not yet know about the virtual hosts. All the dynamic PHP pages should point to the correct place, because these requests are being handled by Apache.

In other words, test by browsing to the following, replacing the examples with your domain names:

* x.x.x.x/static_test.html Should display “ip static”
* x.x.x.x/dynamic_test.php Should display “ip dynamic”
* example1.com/static_test.html Should display “ip static”
* example1.com/dynamic_test.php Should display “example1.com dynamic”
* example2.com/static_test.html Should display “ip static”
* example2.com/dynamic_test.php Should display “example2.com dynamic”

So we are all ok with Apache and dynamic pages. We just need to fix Nginx and the static pages.


#### Setting up virtual hosts (Server Blocks) in Nginx
Let’s browse to out Nginx config files for out sites-available:

{% highlight bash %}
cd /etc/nginx/sites-available/
{% endhighlight %} 

Remember we had one config file here called ip for all of our requests. We need to create a new one for our two example domains. We’ll do example1.com first:

{% highlight bash %}
sudo cp ip example1.com
sudo nano example1.com
{% endhighlight %} 

You’ll need to change the following:

* Remove default_server from the line listen 80; You can only have one default server config, which is the file ip
* Update the root path root /var/www/example1.com/public/
* Add the server name and aliases: server_name example.com www.example.com;

Repeat for example2.com

Now reload the config files for Nginx:

{% highlight bash %}
sudo /etc/init.d/nginx reload
{% endhighlight %} 

And check that the following resolve correctly:

* x.x.x.x/static_test.html Should display “ip static”
* x.x.x.x/dynamic_test.php Should display “ip dynamic”
* example1.com/static_test.html Should display “example1.com static”
* example1.com/dynamic_test.php Should display “example1.com dynamic”
* example2.com/static_test.html Should display “example2.com static”
* example2.com/dynamic_test.php Should display “example2.com dynamic”

## Conclusion
Your webserver should now be configured to serve pages using Nginx, with Apache helping out in the background for any dynamic pages. If you want to add a new domain, you can just create the directory structure, add the configs for Apache and Nginx and reload the servers!