---
layout: post
title:  "Arrow buttons insert characters in VI text editor"
categories: Linux
cover:  "/assets/posts/2017/vim.jpg"
---

I have encountered this issue loads of times before, so thought it would be worth a quick “note to self” for the future. There are some occasions when I log onto a remote machine through SSH and need to edit text files. On some machines, when using the VI text editor, pressing the arrow keys when in INSERT mode will delete the current line and replace it with an arrow key (left=D, right=C, up=A, down=B).

Usually when this happens I give up, hit “:q!” and use another editor such as nano. But sometimes nano isn’t available. Here is how to fix the issue:

Edit the Vim configuration file (this will create one if it doesn’t exist):


{% highlight bash %}
vi ~/.vimrc
{% endhighlight %} 

Then add the line `set nocompatible` to the top of the file before saving and quitting `:wq`

You should now be able to use Vi without any stress (unless you’re more of an Emacs person)




 
