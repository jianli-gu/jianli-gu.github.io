---
layout: post
title: Upgrade Python3 and Setup Default on Mac with Homebrew
categories: [Mac]
tags: [Python, Mac, Homebrew]
modified: 2020-08-20
---

I have been working with Python3.7 for a long time on Mac. As Python3.8 has already been stable,
it's time to move forward to it. We'll use *Homebrew* here,

### Install Python3

Homebrew proivdes a formula for Python3.x (python@3.x), to install it
```bash
$ brew install python3
```

### Upgrade Python3

If you installed sometime before, and just want to upgrade, then try:
```bash
$ brew upgrade python
```

### Switch Python

To confirm the version of Python 3 on Mac now,
```bash
$ python3 --version
Python 3.7.6
```

Why not the latest version of Python 3.8? Because of the symlinks are still the old ones,
we'll need to switch it to the latest version, like this
```bash
$ brew switch python 3.8.5
```

Now, to check the version again,
```bash
$ python3 --version
Python 3.8.5
```

Hope the notes can help. Happy upgrading!