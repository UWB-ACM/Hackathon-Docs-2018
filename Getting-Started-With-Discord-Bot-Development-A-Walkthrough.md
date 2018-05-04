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
only the server has to.



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
