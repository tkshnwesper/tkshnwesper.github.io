---
author: saurabh
comments: true
date: 2013-04-25 16:24:00+00:00
layout: post
link: https://blog.cryf.in/index.php/2013/04/25/basic-irc-internet-relay-chat-commands/
slug: basic-irc-internet-relay-chat-commands
title: Basic IRC (Internet Relay Chat) Commands
wordpress_id: 14
categories:
- Hacks
---

Internet Relay Chat is a protocol for instant messaging, and file transfer. It supports both private messaging as well as group chat. Multiple clients on multiple platforms enable users to use IRC. It forms an ideal chat network over the internet. People can host their own servers to run IRC, or join channels on existing servers.**
****Connecting to a server:
**/server <server_name>
Example:
/server irc.freenode.net






**Joining/Creating a channel:
**/join <channel_name>
Example:
/join #help






**Registering your nick name:**




/msg nickserv register






Example:
/msg nickserv register pass123 mymail@gmail.com

**Registering your channel **(on most servers, you can register a channel only after your nick name is registered)**:
**/msg chanserv register <channel_name>
Example:
/msg chanserv register #help iloveirc IRC Help

**Changing your nick name:
**/nick <new_nickname>
Example:
/nick samurai123

**Leaving a channel:
****/**part


