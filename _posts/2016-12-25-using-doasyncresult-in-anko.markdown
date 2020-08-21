---
author: saurabh
comments: true
date: 2016-12-25 20:34:57+00:00
layout: post
link: https://blog.cryf.in/index.php/2016/12/25/using-doasyncresult-in-anko/
slug: using-doasyncresult-in-anko
title: Using doAsyncResult in Anko
wordpress_id: 345
categories:
- Hacks
---

I was stuck on this for a little while since it wasn't updated in the documentation and also since I am still a newbie at Kotlin ;)

Some of the description has been already added in the code, as you can see. Let me explain the premise of this solution in any case. The problem arose because I had to perform a network task in the `onCreate` method and as you know we can't normally perform a network task in the `uiThread`. Luckily, Anko provides an elegant solution to this without having the need to implement an `AsyncTask` interface. `doAsync` runs whatever task you give it but does not return anything. However, although `doAsyncResult` is similar to `doAsync` it can additionally return an object.



A Merry Christmas to you!
