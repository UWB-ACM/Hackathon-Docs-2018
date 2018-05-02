---
title: Installing Software
layout: topic
categories: guide
---

Installing software that you think you'll need is a great way to prepare for a
hackathon, since time is precious.

This guide lists where to go to install software that you may want for the event.

# Python

[Python3 is the way to go for developing new software, since Python2.7 will no longer be maintained past Jan 1, 2020.][rip-py-2.7]

### Which version do I install?

Either Python3.5 or Python3.6 are usually fine. Most linux distros will already
have 3.5 installed.

## Windows and Mac

[The latest version of Python3.X can be downloaded from it's official downloads page.][python-downloads]

## Linux

You very likely already have a version of Python 3 installed. You can check using
`python --version` or `python3 --version`.

Otherwise, [you can also find installation instructions on the Python website.][python-downloads]

[rip-py-2.7]: https://pythonclock.org/
[python-downloads]: https://www.python.org/downloads/

# C# / Installing Visual Studio Community

Visual Studio is Microsoft's IDE for Windows and Mac that supports many types
of projects. If you are working with C#, it's your best option.

[You can download the community version here.][visual-studio-community]

## C# on Linux‽

Yes, you can develop and use C# on Linux, with a few different options.

- [Monodevelop][monodevelop], for working with the Mono framework.
- [Jetbrains Rider (recommended)][rider], for working with Mono _and_ .NET Core frameworks.
  Free with [the JetBrains Student license][jetbrains-student].
- [.NET Core Framework][netcore]. You'll want to install this on your servers so you can run
your C# applications.

[visual-studio-community]: https://www.visualstudio.com/vs/community/
[monodevelop]: http://www.monodevelop.com/
[rider]: https://www.jetbrains.com/rider/
[netcore]: https://www.microsoft.com/net/learn/get-started
[jetbrains-student]: https://www.jetbrains.com/student/

# Java

You should aim to use Java [JDK 8][java-jdk-8], although [JDK 10][java-jdk-10] is currently available.

- [IntelliJ IDEA (recommended)][intellij-idea], fully-featured IDE.
- [Eclipse Oxygen][eclipse-oxygen], fully-featured IDE.
- [Gradle][gradle], build automation tool.

Although **Optional**, these tools are recommended for any Java-based project.

- [FindBugs][findbugs], static analysis tool for debugging.
- [PMD][pmd], another static analysis tool for debugging.
- [Checkstyle][checkstyle], static analysis tool to enforce a coding standard.
- [JUnit][junit], test automation tool.

[java-jdk-8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[java-jdk-10]: http://www.oracle.com/technetwork/java/javase/downloads/jdk10-downloads-4416644.html

[intellij-idea]: https://www.jetbrains.com/idea/
[eclipse-oxygen]: https://www.eclipse.org/downloads/
[gradle]: https://gradle.org/

[findbugs]: http://findbugs.sourceforge.net/
[pmd]: https://pmd.github.io/
[checkstyle]: http://checkstyle.sourceforge.net/
[junit]: https://junit.org/junit5/

# Misc

[The GitHub student pack lists many software tools and services that are licensed for students at no cost or heavily discounted.][student-pack]

[student-pack]: https://education.github.com/pack
