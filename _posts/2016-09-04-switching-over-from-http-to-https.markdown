---
author: saurabh
comments: true
date: 2016-09-04 13:39:47+00:00
layout: post
link: https://blog.cryf.in/index.php/2016/09/04/switching-over-from-http-to-https/
slug: switching-over-from-http-to-https
title: Switching over from http to https
wordpress_id: 326
categories:
- Hacks
---

Hip hip hurray! I recently made the switch using a popular and open source service known as [Let's Encrypt](https://letsencrypt.org/). Its service provides an SSL layer over the connection so that information sent to the server will not be understandable by the attacker even if they listen on the network. Well, to be honest I've been wanting to do this for a long time, but I did not know about this free service before! It works really well and a couple of tutorials helped me set up my Nginx Wordpress installation for https.





So yeah, those lines above did it for me. But before you add them to your Nginx configuration file, follow the steps given on the Let's Encrypt site. You need to start by installing `certbot` on your system, then create certificates and keys. Since these certificates expire after 90 days, you also need to renew them periodically. But rest assured, the setup was easier than I had expected, you should certainly give it a try. Not to mention that it's free :)
