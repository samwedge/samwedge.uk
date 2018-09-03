---
layout: post
title:  "WordPress Permalinks Not Working (404 Not Found)"
categories: Linux
cover:  "/assets/posts/2018/wordpress.jpg"
---

I recently migrated this WordPress website between servers. It was fairly straightforward: copy the database, copy the files, transfer the domain etc. However, one (big) issue that I encountered was that with the exception of the homepage and the admin dashboard, every page resulted in a 404 Not Found. I checked my Permalink settings in the dashboard – this was set as it should have been (custom structure set to `posts/%postname%`). This was indeed the format pages were trying to access, so it should have been fine.

As a quick test, I turned off my custom permalink and set it to Plain and bing – everything now worked, albeit with a permalink structure that I was not happy with (both because it looked less readable and I didn’t want to break my links – I’ve managed to keep them since I migrated to WordPress from Grav a few years ago). So I set my Custom Structure back on and went hunting for the cause of the issue.

Changing the permalink structure amends the `.htaccess` file in the root wordpress directory. If the permissions for this file are set incorrectly then WordPress can’t write to it. I checked the permissions and there was no issue. The contents of the file contained the correct Apache Directives set by WordPress.

It was as if Apache was ignoring my `.htaccess` file. hmm… let’s have a quick google…

Seems as though Apache is configured by default to prevent its settings from being overridden by the `.htaccess` file. So I went into my site’s configuration file and added:

{% highlight apache %}
<Directory /var/www/path/to/website/ >
    AllowOverride All
</Directory>
{% endhighlight %} 

I then restarted Apache and checked my website – everything worked 🙂

