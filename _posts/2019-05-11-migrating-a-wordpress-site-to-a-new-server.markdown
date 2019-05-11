---
layout: post
title:  "Migrating a wordpress site to a new server"
categories: Linux
cover:  "/assets/posts/2019/bird-migrate.jpg"
---

I wanted to move some wordpress sites from an old server to a new one and wanted it to be as seamless as possible.
I really didn't want to use an automated plugin - I've tried them in the past and they either don't work (either badly coded or some missing PHP library)
or they require a paid subscription for some ongoing backup service that I don't want.

I decided to do it the old fashioned way, by logging on to the servers via ssh. Incidentally, the steps below can also be used
to provide a backup and restore process.

The following assumes that you are connected via ssh as the root user. If not, you may need to amend some of the commands. It also assumes you've configured your webserver and MySQL.

Note that any changes to the Wordpress site in between taking the backup and switching to the new server will be lost - obviously, as they aren't backed up.

### Backup the old files
Create a location for the backup
{% highlight bash %}
mkdir ~/mysite_backup
{% endhighlight %}

Take a dump of the mysql database to the backup folder, replacing `hostname`, `username` and `database_name` with the relevant information and entering the password when prompted
{% highlight bash %}
mysqldump -h hostname -u username -p database_name > ~/mysite_backup/db.sql
{% endhighlight %}

Copy in the wordpress files, printing out progress as it goes via the `verbose` flag
{% highlight bash %}
cp -v path/to/wordpress/folder ~/mysite_backup/
{% endhighlight %}

Then we'll create a gzipped archive. The options here are:
* z - use gzip compression
* c - creates an archive
* v - verbose, tells us what's going on while the archive is being generated
* p - preserve permissions
* f - lets us specify a filename to write the archive to
{% highlight bash %}
tar -zcvpf backup.tar.gz ~/mysite_backup
{% endhighlight %}

### Copying the backup file to the new server
You can use `scp` to securely copy the backup file to the new server. This may take a while if your Wordpress installation is particularly large.

{% highlight bash %}
scp -v ~/mysite_backup/backup.tar.gz root@new.server.ip.address:/path/to/copy/file/on/new/server
{% endhighlight %}

Then delete the backup files from the old server.
{% highlight bash %}
rm -rf ~/mysite_backup
{% endhighlight %}

### Restoring the backup in the new location
Unpack the backup file. The options here are:
* x - extracts an archive
* v - verbose, tells us what's going on while the archive is being generated
* z - use gzip compression
* f - lets us specify a filename to read the archive from
{% highlight bash %}
cd /path/to/backup/file/on/new/server
tar -xvzf backup.tar.gz
{% endhighlight %}

Put the Wordpress files wherever they need to go (determined by your webserver config).
* r - recursive
* p - preserve permissions 
{% highlight bash %}
cp -rp backup/mysite_backup/* /path/to/wordpress/installation/
{% endhighlight %}

Create the database from the backup sql file, replacing the `hostname`, `username` and `database_name` with the correct info, and entering the password when prompted
{% highlight bash %}
mysql -h hostname -u username -p database_name < backup.sql
{% endhighlight %}

Then clear away the backup files
{% highlight bash %}
rm -rf backup/*
{% endhighlight %}

Then set the owner `chown`, group `chgrp` and permissions `chmod` in accordance with your setup. The following is an example, and not a recommendation. You need to choose something based on your installation.
{% highlight bash %}
chown -R root path/to/wordpress/installation
chgrp -R www-data path/to/wordpress/installation
chmod -R 644 path/to/wordpress/installation
{% endhighlight %}

Make sure you restart your webserver. In the case of Apache:
{% highlight bash %}
service apache2 restart
{% endhighlight %}

### Testing things out
If you're feeling cavalier and you've already configured your webserver to point to your new files, go ahead and change the DNS settings now.
But if you're a little more cautious, you can use a spare domain or your server's ip address and configure your webserver to direct traffic to the new Wordpress site.
One problem with this is that Wordpress redirects traffic to its own domain, so you need to fool it into thinking the ip address or spare domain is its correct address.
This is an option normally changed via the admin panel, but due to the redirect, attempting to change this value via the admin panel will update the old Wordpress installation
even if you type in the address of the new installation. To get around this, you can temporarily hard-code the value. Find the `wp-admin/wp-config.php` file and add he following lines near the beginning:
{% highlight php %}
define('WP_HOME','http://your_test_url');
define('WP_SITEURL','http://your_test_url');
{% endhighlight %}

Test away. If all is good, remove the two lines from your `wp-config.php` file and update your DNS records.
