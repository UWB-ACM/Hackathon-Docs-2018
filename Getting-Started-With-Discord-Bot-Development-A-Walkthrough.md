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

Navigate to the 'My Apps' page in the top left of the documentation page.
Click on the 'new app' button.

![Discord developer apps dashboard 'new app' button.][new_app]

Fill out the 'create app' window.

![Discord create app window][create_app]

If all goes well, you should see a screen like this:

![Discord app confirmation screen][app_confirmation]

Discord supports multiple types of applications, but all we carry about
are 'Bot Users'. Scroll down and click on the 'Create a Bot User' button.

![Creating a bot user button.][bot_user]

Now, you should see the bot details section. This contains your bot's client
ID and Bot Token. **Do not reveal this token publicly!**
[Click here for an explanation why.][dont-leak-tokens]

![Bot details section][bot_details]

**If you accidentally leak your token**, go back here to generate a new
one by clicking on 'Generate a new token?'. Don't worry, the one here
is already invalidated.

Now, scroll up and find the 'Generate OAuth2 URL'. You'll need this to
invite your bot to your server.

![Generate OAuth2 URL button.][invite_link]

Copy the URL that is generated from this tool, with the default permissions
for a bot. Paste it into a new tab. You'll see a prompt to add it to one
of your servers. Create a testing server to use, if you haven't already,
then refresh the page and add the bot there.

**When developing Discord bots, it's really useful to have a testing server
with just you and the bot in it.** You can only add bots to servers that you
have the 'Manage Messages' permission enabled.

If all goes well, you should see that the bot has joined your server.
It will be offline, and have a blue `BOT` tag next to it's name.

![Bot that has joined a test server.][join_server]

[new_app]: img/discord_new_app.png
[create_app]: img/discord_create_app.png
[app_confirmation]: img/discord_app_details.png
[bot_user]: img/discord_bot_user.png
[bot_details]: img/discord_bot_details.png
[invite_link]: img/discord_invite_link_generator.png
[join_server]: img/discord_join_server.png
[dont-leak-tokens]: Token-Management-Common-Practices.md

# Step 2. Set up your Development environment

This document is not a comprehensive guide for doing first-time setup
and installation for Python.
[A brief guide can be found here.][installing-software]

[This walkthrough will use code from the Discord Python example on the UWB-ACM GitHub page, as found here.][acm-dpy]

## Downloading the Example Bot

You'll want to clone the example repo to your local machine to get started.
This guide assumes that you have git installed in your command line. If
you are using a GUI, follow along using the procedures that the GUI uses.

```bash
# Navigate to a projects directory, if you use one
cd ~/Git
# clone the example bot
git clone https://github.com/UWB-ACM/Discord-Python-Example
```

Alternatively, you can download the source from GitHub as a `.zip` file.

## Installing Prerequisites

The example bot needs some software installed first, specifically the
Discord library that we will be using.

The instructions for installing them are listed on the project page, but
the command to install the requirements using `pip` is:

```bash
python3 -m pip install -r requirements.txt
```

or on Windows

```
py -3 -m pip install -r requirements.txt
```

If the command fails, you may need to run the command again as
root/Administrator.

[installing-software]: Installing-Software.md
[dpy-install]: http://discordpy.readthedocs.io/en/rewrite/intro.html#installing
[acm-dpy]: https://github.com/UWB-ACM/Discord-Python-Example

# Step 3. Get the bot online


# Appendix - What is the bot doing?

The code is commented to provide an explanation of what's going on, but
here's a walkthrough of what it's doing.

### Main

```py
import discord
from discord.ext import commands
```
Imports the `discord.py` library, so that we can use it in our code.

```py
config = configparser.ConfigParser()
with open('config.ini') as config_file:
config.read_file(config_file)
```
Parses our `config.ini` file that was created in step 3. We use this
later on when logging in.

```py
client = commands.Bot(command_prefix='>>', description='https://github.com/UWB-ACM/CSSBot_Py')
```
Defines a new `Bot`, with the specified prefix before commands and some
description.
Command prefixes are the characters at the start of messages that indicate
that the message is a command directed towards this bot.
Avoid common prefixes like `.`, `/`, and `!`, because those are used very
often.

```py
default_extensions = ['cogs.basic']
if __name__ == '__main__':
    for extension in default_extensions:
        try:
            client.load_extension(extension)
        except Exception as e:
            print('Failed to load extension ' + extension, file=sys.stderr)
            traceback.print_exc()
```
One feature provided to us by the discord.py library is a feature they call
'cogs'. Cogs are plug-ins that are loaded at runtime that have some
functionality. They can contain commands and respond to events.
Keeping groups of commands modular can greatly improve the readability
and maintainability of your code base.

```py
@client.event
async def on_ready():
    # print some stuff when the bot goes online
    print('Logged in ' + str(client.user.name) + ' - ' +
          str(client.user.id) + '\n' + 'Version ' + str(discord.__version__))
    # a good way to let users know how to use the bot is by providing them with a help method
    # only way this can do them any good is by letting them know what the help command is
    await client.change_presence(activity=discord.Game('Try >>help'))
```
This method, marked as a `client.event` waits until the bot is 'Ready'.
Discord bots are considered 'Ready' once they are connected to all of the
servers ('guilds') that they are members of.
Once the bot is connected, it prints out it's name, user ID and the version
of the library it's using.
At the end, the bot changes their status to say that they are 'playing'
"Try >>help".
By making the status of your bot show the help command, it can give users
a hint as to how they can use the bot.

```py
client.run(config.get(section='Configuration', option='connection_token'),
                      bot=True, reconnect=True)
```
This last snippet parses the config file that we loaded earlier for the
connection token, and feeds it into the library-provided `run` method.
The bot will start the process of connecting and should come online
momentarily.
The bot will remain running for as long as the program is running.

### Basic Cog

Remember that 'cog' we loaded called `'cogs.basic'`?
Here's an incomplete walkthrough of that.

```py
import discord
from discord.ext import commands

class BasicCog:
    def __init__(self, bot):
      self.bot = bot
```
Here, we import the libraries again. In the constructor for `BasicCog`,
we pass a reference to `bot`. `bot` is our client, which handles
all of the interaction for us.

```py
@commands.command()
async def ping(self, ctx):
  await ctx.send("Pong!")
```
This is the most basic command there is. The attribute `@commands.command()`
marks this function as a command for the bot. When the bot sees the
message `>>ping`, it will reply back with the text "Pong!".
The `ctx` variable contains the context where the message was received,
including references to the author, channel, and server (guild).

```py
@commands.command(name='echo')
async def echo(self, ctx, *, message: str='_echo_'):
  await ctx.send(ctx.author.mention + ' : ' + message)
```
This is the `>>echo` command. When the user types `>>echo some text`, the
bot will reply back with `@Author : some text`.

[The way that parameters are handled by the discord.py library is explained in their documenation.](http://discordpy.readthedocs.io/en/rewrite/ext/commands/commands.html#parameters)

```py
def setup(bot):
  bot.add_cog(BasicCog(bot))
```
This method is the most important in this file,
all 'cogs' need this function defined, as that is their
entry point to be loaded. It creates a new `BasicCog` object, as defined
earlier in the file, and passes it to the bot. It's given a reference to
the bot so that this cog can interact with the client.

# What now?

After you have completed all these steps, you should have a functional
Discord bot that can respond to a basic command. You can build on the
functionality given to you by the library and the barebones example
to develop more complex behavior.

Happy hacking!

[discord-docs]: https://discordapp.com/developers/docs/intro
[wikipedia-api]: https://en.wikipedia.org/wiki/Application_programming_interface
