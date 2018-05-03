---
title: Token Management Common Practices
layout: topic
categories: guide
---

# What are Tokens?

Tokens are strings that allow applications to authenticate who belongs
to a connection. Think of them like a username and a password combined.
For most actions that users (or bots) do on websites, it will send
your token along with it.

# Why should I keep them secret?

In short, think of them as a combined username and
password, that allows users and bots to use any
part of the API.

## An Example, Using Discord

When you log into Discord, the login form sends your credentials to
the API endpoint `/login`. The body of this HTTP POST Request contains
the following (some stuff left out):

```json
{
  "email": "mark@givemethezu.cc",
  "password": "YourPasswordInPlainText",
}
```
_(This is also why HTTPS is so important.)_

Storing your login credentials and sending this data for _every_ network
transaction is impractical, and a really, really, bad idea.
Tokens help solve this issue, they act as a single-use/short term password,
and is unique across devices. (When you choose to "Save Your Password", really
what you are doing is storing the login token in your browser storage so that
you don't have to get a new one.)

Generally, these tokens can be invalidated manually,
either by "forgetting this device", resetting a password, or not using a
token for a while.

After a successful (user) login, the `/login` endpoint returns some data
that looks like this:

```json
{
  "token": "lkjsdflkjsdpfoaijasdfthisisafaketokenpoijqwpoijpofijasfpoiasjdfpoijwef"
}
```

Now, the user is logged in, and can access the app like they normally can.
Whenever a request to the API is made, the `authorization` field in
the request headers is set to the user token.
The email and password combination are only dealt with when you log in, and
never again.

Keep in mind, this is the only method of authentication once you are logged
in to the app. This means _all user functions_ can be done with this function
(excluding ones that ask you to re-verify).

**If someone has access to your user token, they are effectively logged into
your account, and can do anything you normally can.**

This includes:
  - Sending Messages
  - Reading Messages
  - Changing Account Information

Ensuring that you don't leak user tokens is very important.
Many APIs use tokens for both bots and users, and in generally the same way.
One critical difference is that **bot tokens (generally) do not expire**, so
if your token gets leaked, it can be used indefinitely until you manually
reset the tokens.

This poses a serious risk if you run a bot that has access to many user's data,
or has permissions to modify user data.

**TLDR** This is what happens to your users and bots if your token gets
leaked:

<img src="https://media.giphy.com/media/HhTXt43pk1I1W/200.gif" width="200px"/>

That being said, how can we ensure that this _doesn't_
happen?

# What NOT To Do - How Tokens Get Leaked

There are some easy mistakes that can lead to leaking
your tokens. Depending on how the tokens are leaked,
malicious users or bots can find these and start to
abuse them.

"Hardcoding" a token means that somewhere in your code you have
the bot token stored. If you decide to use any version control, this token
is permanently exposed, and you should change it immediately.

- Don't do this: `token = 'lkjasdlfkasdfljksdfa'`
  - Don't upload this to a public Git repo.
  - Don't take a screenshot of this and post it somewhere.
- Don't set up your IDE to pass a token as an argument, then include
the IDE project files in your code base.
- Don't add your `config.ini` files to version control. (Use a `.gitignore`!)

# How To Manage Connection Tokens

Ok, here's how to _actually_ maange connection tokens. Each section will have
a link at the end with an example of how these are done.

## Option A - Configuration Files (Recommended)

Configuration files are probably the way to go for most applications.
They provide an easy way to set tokens, and can be re-used for other
configuration settings.

When developing your bot, you can use two sets of tokens: one for your
development bot for testing, and one for you production bot.
By either switching out configuration files, or modifying the contents of them
you can easily switch between instances of your bot.

Example bots that use a configuration file are:

- [Twitter-Python-Example][tweepy]
- [DiscordBot_Java][discordjava]
- [Discord-CSharp-Example][cssbot]
- [Discord-Python-Example][discordpy]

## Option B - Environment Variables

Environment variables are another way to set tokens and other values,
but are arguably less secure and can be less useful.

Pros:
  - Easy to set.
  - Don't need to deal with reading a file.

Cons:
  - Other applications can read environment variables.
  - Running multiple instances of bots on the same machine cannot be done (cleanly).

An example bot that uses environment variables is:

[Spotify-Python-Demo][spotipy] (because of features provided by the library)

[tweepy]: https://github.com/UWB-ACM/Twitter-Python-Example
[discordjava]: https://github.com/UWB-ACM/DiscordBot_Java
[cssbot]: https://github.com/UWB-ACM/Discord-CSharp-Example
[discordpy]: https://github.com/UWB-ACM/Discord-Python-Example
[spotipy]: https://github.com/UWB-ACM/Spotify-Python-Demo
