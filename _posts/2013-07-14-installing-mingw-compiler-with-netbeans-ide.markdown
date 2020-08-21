---
author: saurabh
comments: true
date: 2013-07-14 07:17:00+00:00
layout: post
link: https://blog.cryf.in/index.php/2013/07/14/installing-mingw-compiler-with-netbeans-ide/
slug: installing-mingw-compiler-with-netbeans-ide
title: Installing MinGW Compiler with NetBeans (IDE)
wordpress_id: 79
categories:
- Hacks
---





 	
  * Download and install the MinGW Compiler.

 	
  * In the NetBeans IDE, click on _Tools_>_Options_.

 	
  * Go to the C/C++ tab.

 	
  * Click on _Add_ under _Tool Collection_.

 	
  * Select the directory where MinGW is installed. It will in most cases be _C:/MinGW/bin_.

 	
  * Now download and install [MSYS](http://downloads.sourceforge.net/mingw/MSYS-1.0.11.exe). Identify the directory in which MinGW is installed as _C:/MinGW_ to the MSYS installer.

 	
  * Open the file browser next to the _Make Command_, and select the _make.exe_ in the directory where MSYS is installed.

 	
  * Restart the IDE.



