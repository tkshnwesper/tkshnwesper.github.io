---
author: saurabh
comments: true
date: 2014-06-07 21:09:22+00:00
layout: post
link: https://blog.cryf.in/index.php/2014/06/08/how-to-install-flash-player-for-chromium/
slug: how-to-install-flash-player-for-chromium
title: How to install flash player for Chromium
wordpress_id: 147
categories:
- Hacks
---

I am currently using Debian "Wheezy", and I've been using it for a while now. Recently, I realized that I was browsing the web without Adobe Flash Player installed on my system. Although, I did a normal apt-get install flashplugin-nonfree things did not work. So I searched a little more and found Google's custom flash player called Pepper Flash Player.

Installing PFP is a bit of a pickle unless you have already added backports to your repositories. To add the backports repositories, you need to add a line to sources.list file which is present in  /etc/apt/.

Add this line to sources.list,
deb http://ftp.debian.org/debian wheezy-backports main contrib non-free

Now update your package list,
apt-get update

Almost done, now install package pepperflashplugin-nonfree,
apt-get -t wheezy-backports install pepperflashplugin-nonfree

Done!
