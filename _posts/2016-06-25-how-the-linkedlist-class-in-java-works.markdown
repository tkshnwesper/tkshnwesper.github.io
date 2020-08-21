---
author: saurabh
comments: true
date: 2016-06-25 11:10:50+00:00
layout: post
link: https://blog.cryf.in/index.php/2016/06/25/how-the-linkedlist-class-in-java-works/
slug: how-the-linkedlist-class-in-java-works
title: How the LinkedList class in Java works
wordpress_id: 298
categories:
- Hacks
---



I will explain how to perform operations like add, push, pop, poll, etc on LinkedList and what they mean. At first, it may seem a bit confusing, but I hope this article resolves a few conflicts.

A simple LinkedList of Strings declaration and definition looks like this:




There are several ways to add new elements to a LinkedList, but they mean and do different things. I will show you how each function works one by one.



#### add


One of the first methods you might encounter to add an element to a LinkedList is add. You can make use of add function in this manner:




Now let's try to do the same with push!



#### push






We can conclude from this that add puts the element at the first position and then moves on to further positions. push on the other hand inserts elements quite like a stack. The element that is inserted first is the last when we traverse the linked list.

Let's look at ways to fetch elements from the LinkedList. For this, we need to assume that the linked list already has some elements. Assume that for the subsequent examples, the LinkedList contains elements added using the add method like above.



#### pop


Just a reminder that elements are added in the same fashion as in add.



Since pop and poll are nearly the same (poll returns null when the list is empty whereas pop throws an exception) I will not discuss poll.



#### pollFirst


Just a reminder that elements are added in the same fashion as in add.



As can be seen, pollFirst and pop function produce similar output.



#### pollLast


Just a reminder that elements are added in the same fashion as in add.



And finally, pollLast!
