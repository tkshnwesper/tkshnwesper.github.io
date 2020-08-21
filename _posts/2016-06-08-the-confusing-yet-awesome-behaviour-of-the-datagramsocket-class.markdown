---
author: saurabh
comments: true
date: 2016-06-08 10:26:44+00:00
layout: post
link: https://blog.cryf.in/index.php/2016/06/08/the-confusing-yet-awesome-behaviour-of-the-datagramsocket-class/
slug: the-confusing-yet-awesome-behaviour-of-the-datagramsocket-class
title: The confusing yet awesome behaviour of the DatagramSocket class
wordpress_id: 289
categories:
- Hacks
---

One of the recent projects that I am doing involves using UDP datagrams for communication. The DatagramSocket class in Java enables me to use that feature. At first, I wasn't aware of all the features of the class, but as I started coding and explored different methods of using that package, several more things became clear to me. Well, firstly the documentation and tutorials that I looked up to understand how to use the class gave me a very minimalistic approach to using it. So, let me clarify how this class manages the network and how you can use this.

Let's start with the constructor. For a minimalist working server, you must provide a port (otherwise packets might not get delivered to you). Basically, the DatagramSocket class takes care of listening to a port and sending data from that same port (if needed). It does not automatically send ACKs when a packet arrives. It listens, halts the process till data arrives.

A client on the other hand does not require a port to be specified in the constructor for minimalist working. Why? Because we are assuming that the client is not listening for any data (for the time being). It only sends the provided data to a specific port specified by an InetAddress. Now there is an important thing to understand about all this. If a port is not specified by the client, it randomly chooses one. Not specifying a port does not mean that a port is not required by the client to send the data! When the DatagramPacket arrives at the server's end, the server can find out from the packet several things like the InetAddress of the source and the port through which the packet was sent. This allows the server to send data to that source through it's address and on the port that it's using.

The conclusion that can be drawn from these facts is pretty cool. A thread that's listening on a port can receive packets from multiple sources, and in the same way a thread that's sending packets can send them to different hosts!

**UPDATE:
**The real important thing to remember is that you cannot use the same socket simultaneously for sending and listening. If a socket is listening on a certain port, you need to either make it stop listening and send your packet away or you need to open another socket to do so.

Of course, it is not possible to listen on multiple threads using just one socket. For this reason, one of a good architecture designing techniques for such a mechanism is as follows:



 	
  * Listen on one socket

 	
  * Send packets using another socket


