---
title: Getting Started With Discord Bot Development - A Walkthrough
layout: topic
categories: guide
---

### Note

This guide walks through the steps for setting up a Discord bot
that is written in Python, and running on Linux (or Windows with WSL). Many
of the key concepts are the same for different platforms and languages.
You don't have to know Python, but it will help for this example.

There are also some different ways that the Discord API can be used, but this
document will

# How Bots (and Users) Work

In the case of Discord and many other services, bots interact with the Discord
'back-end' in a very similar way to how regular users do.

When regular users use Discord, they interact through a 'front-end' client.
Discord supports many types of clients, including webpages, native desktop
apps, and native phone apps. These clients all have to behave in the same way
so that the users get a cohesive and consistent experience, but they cannot
share the exact same code.

But Discord _can_ and _does_
use the same [API (Application Programming Interface)][wikipedia-api]
across all of their devices. When any of the clients connect and
communicate to the Discord servers, they are going through this API
which is responsible for handling the interaction between users.
This way, the client isn't responsible for handling any complex operations,
only the server does.

![Relationship between different types of clients and the Discord API][dapi-diagram]

Bots are just another type of client to Discord. They do have
their own set of rules and restrictions, but they interface with the API
through the same methods. All of the endpoints and operations that can be
done with the API are listed in the documentation, if you want to see what
is happening 'under the hood'. You can also view the network transactions
in the 'Network' tab in your browser.

Bots do have some restrictions applied to them, though. They can do
some set of things differently than normal users, and cannot do some things
normal users can.

[dapi-diagram]: img/discord_api_diagram.png

# API vs Library

When you are looking to integrate another service in your application, you
may hear the terms 'API' and 'Library' thrown around interchangeably.
In the context of this documentation, we will use the following definitions:

- **API / Application Programming Interface**: The interface that
  is used to directly communicate with a service or application.
  In the case of Discord, Twitter, Spotify, and many others,
  these are implemented using
  [HTML REST communications.][spring-understanding-rest]
  The HTML protocol is just text, and works with nearly every language on
  nearly every platform.

- **Library**: Any package or assembly of code which provides some set of
  utility or functionality to the library user. There are many types of
  libraries that can do any number of functions.
  For these examples, 'Libraries' will refer to the software that
  handles the communication with an API for you.
  Libraries are written in a specific language and only work on a certain
  set of platforms.

## Why should I use a library?

The biggest difference between APIs and Libraries for developers (you)
is the functionality and the ease of use that you get out of it.

Let's use an example.

**API**

Here's what I have to send to the API to send a message:

```
POST https://discordapp.com/api/v6/channels/123412341234/messages
Authorization: YourUserTokenGoesHere
Body:
{
  'content': 'this is my message',
  'tts': false  
}
```
By the way,
[Postman][postman] and `curl` are both great ways to test out APIs.

I'd have to figure out the best way to send that data in whichever
language I work with, and handle it all myself.

Or...

**Library**

There are tons of libraries for the Discord API. In this example we will
use [discord.py][dpy] for Python, but you can really use whatever language you
want. Here's the equivalent process using that library.

```python
channel = client.get_channel(123412341234)
await client.send_message(channel, 'This is my message‽')
```

Using a library is a much higher-level and easier approach to interfacing
with APIs. They usually do a lot of the work for you, and can add
features on top.

[postman]: https://www.getpostman.com/
[spring-understanding-rest]: https://spring.io/understanding/REST
[dpy]: https://github.com/Rapptz/discord.py

# Step 0. Prerequisites

Before you can begin making a Discord bot, you'll have to have an account.
Ensure that you have one, and take some time to become familiar with the
service itself. It's hard to develop for services that you don't understand.

# Step 1. Set up a new Bot

[Navigate over to the Discord Developer Documentation.][discord-docs]
(You'll also want to be logged in.) This documentation describes all of their
API features, but don't worry about those right now.

[discord-docs]: https://discordapp.com/developers/docs/intro
[wikipedia-api]: https://en.wikipedia.org/wiki/Application_programming_interface
