---
layout: post
title:  "Adding system-wide environment variables on Linux systems"
categories: Linux
cover:  "/assets/posts/2017/env_vars.jpg"
---

I had a need to set an environment variable which was accessible to some python code so that I could have my code use a different configuration on my home development computer from my webserver.

Setting a variable is easy (just type `export var_name=value`). However, the variable is not accessible outside of that terminal.

You can make it persist by adding the command into your shell profile file (`~/.profile`), but this is only available to the current user.

To make it available to all users, you will need to add it into `/etc/.profile`. However, still, this is not universal, and is only accessible to Bourne-compatible “login” shells.

If you want to persist an environment variable to be available to all users, all shells (“login or non-login shells, Bourne-compatible or non-Bourne-compatible shells) and graphical programs, then you can edit /etc/environment and add the line:
`var_name="value"`

This environment file gets read by PAM (Pluggable Authentication Module). For completeness, you could also add a line to the PAM configuration file, but this is dangerous as if you mess up the formatting then you can prevent all users of the system from being able to log in! But if you want to do this, you would edit `/etc/security/pam_env.conf` and add the line:
`var_name DEFAULT="value"`

Editing pam_env.conf directly is only shown here as there are some tutorials recommending to do this over editing `/etc/environment`. Do not edit pam_env.conf yourself unless you accept the danger!

You will need to restart to make the changes live for all users, as this file gets processed at login time.
