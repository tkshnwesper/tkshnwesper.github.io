---
author: saurabh
comments: true
date: 2014-05-22 00:12:25+00:00
layout: post
link: https://blog.cryf.in/index.php/2014/05/22/scraping-animax-india/
slug: scraping-animax-india
title: Scraping Animax India!
wordpress_id: 133
categories:
- Hacks
---

Since Animax India Community (AIC) is going to be taken down soon, I decided to make script to scrape what I wanted to keep. People might look at it as a failure, because it fails to deliver exactly what they want, but to be honest, it doesn't matter.

The amount of knowledge I have gained in the past few days is not bad at all. I should take breaks more often, I am unable to think.

Let's cut to the chase. I stopped developing only because I get a unicode error when I use the Beautiful Soup library and I think some files got corrupted.


<blockquote>

> 
> /usr/lib/python2.7/dist-packages/bs4/dammit.py:231: UnicodeWarning: Some characters could not be decoded, and were replaced with REPLACEMENT CHARACTER.
"Some characters could not be decoded, and were "
> 
> 
</blockquote>


I think I know how this happened but it is probably because my laptop shuts down because of power cuts. My laptop doesn't have a battery because I removed it. It's dead that's why. Well anyway, there have been quite a few power cuts here recently. So yeah, you can figure the rest. If you get any errors let me know.

That's not all, the HTML file that I scrape is full of random meaningless characters. Makes me nauseous by just looking at it.

But here is what I have done. It should work on your computers (hopefully). You need to have the libraries: Beautiful Soup (python-bs4), Mechanize (python-mechanize). Of course, you need to have Python too! I am running it on 2.7.

Note for Windows users:
Download the libraries using this: [https://pip.pypa.io/en/latest/](https://pip.pypa.io/en/latest/) or [https://pypi.python.org/pypi/setuptools](https://pypi.python.org/pypi/setuptools)

Finally, here it is.
[https://mega.co.nz/#!epphnTSK!_0Jr8J1Fu4qaxUXZoa84StSflqRT21LYkEMLA3x5nCE](https://mega.co.nz/#!epphnTSK!_0Jr8J1Fu4qaxUXZoa84StSflqRT21LYkEMLA3x5nCE)

This is how you run it:
Go to the terminal or command prompt, whichever OS you use. Navigate to the location of the file. Execute the following:


python aic_scraper.py


(P.S: Don't forget to set the path variable!)
Have fun!

Update:
Everything works fine now.
