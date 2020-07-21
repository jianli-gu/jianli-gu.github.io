---
layout: post
title: Docker Permission on Ubuntu
categories: [Docker]
tags: [Docker, Ubuntu]
modified: 2020-07-15
---

After docker installation on Ubuntu, you may get an error raised as me below,

```bash
$ docker info
Got permission denied while trying to connect to the Docker daemon socket at
unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.38/info:
dial unix /var/run/docker.sock: connect: permission denied
```

The error message tells you that the current user you're using on Ubuntu has no permission to
access to the unix socket and communicate with docker engine.

How to solve this issue related to permission? Two possible solutions are listed here.

[1] Root user

To run the command as `root` using sudo as a temporary solution.

```bash
$ sudo docker info
```

[2] Docker group

The recommended ways is to add the current user to `docker` group.

``` bash
$ sudo usermod -a -G docker $USER
```

However, the group does not take effect unless the current session is closed,
so please log out of your account and log back again completely.
If the issue still persists, please reboot your system.

Hope it helps!
