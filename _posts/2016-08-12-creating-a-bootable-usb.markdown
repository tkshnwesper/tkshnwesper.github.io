---
author: saurabh
comments: true
date: 2016-08-12 07:45:33+00:00
layout: post
link: https://blog.cryf.in/index.php/2016/08/12/creating-a-bootable-usb/
slug: creating-a-bootable-usb
title: Creating a Bootable USB
wordpress_id: 320
categories:
- Hacks
---

Recently, I had the need to create a bootable USB to install the new LTS version of Ubuntu on my laptop. I stumbled upon a tool called [Rufus](https://rufus.akeo.ie/) which did the trick very well.

But there were several problems along the way, most because there weren't clear instructions on all the sites that I visited in order to learn how to create a bootable USB.

The first thing that you have to do is download the tool, whichever OS you are on. Run it, and browse for the disk image. Select the GPT partition style in the dropdown menu, and now the most important part:

Set _Create bootable disk using _to DD tools!

I was stuck here because I was using ISO image instead. When I tried booting my USB on startup I got a "Missing operating system" message. I got the same message even when I created the bootable USB using the tool provided by Ubuntu!
