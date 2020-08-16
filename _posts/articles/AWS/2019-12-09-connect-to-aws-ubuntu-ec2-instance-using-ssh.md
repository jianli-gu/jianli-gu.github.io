---
layout: post
title: Connect to AWS Ubuntu EC2 Instance using SSH
categories: [AWS]
tags: [AWS, EC2, Ubuntu, ssh]
modified: 2019-12-09
---

The steps below show how to connect to Ubuntu EC2 instance using `ssh`.

###### step 1:
Launch an Ubuntu EC2 instance, and download the key pair - `example.pem`.

###### step 2:
Edit the Inbound of Security Group by adding SSH - Source (My IP).

###### step 3:
Try to connect by using SSH.
```bash
$ ssh -i path/to/example.pem ubuntu@ec2-xxx-xxx-xxx-xxx.compute-x.amazonaws.com
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'path/to/example.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "path/to/example.pem": bad permissions
ubuntu@ec2-xxx-xxx-xxx-xxx.compute-x.amazonaws.com: Permission denied (publickey).
```

###### step 4:
In this case, we need to change the mod of `example.pem`.
```bash
$ chmod 400 path/to/example.pem 
```
Now, we can connect to Ubuntu EC2 instance using `ssh`.
```bash
$ user@host:~$ ssh -i path/to/example.pem ubuntu@ec2-xxx-xxx-xxx-xxx.compute-x.amazonaws.com
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-1051-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue Dec 10 06:14:48 UTC 2019

  System load:  0.0               Processes:           85
  Usage of /:   13.6% of 7.69GB   Users logged in:     0
  Memory usage: 14%               IP address for eth0: xxx.xxx.xxx.xxx
  Swap usage:   0%

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo ".
See "man sudo_root" for details.

ubuntu@ip-xxx-xxx-xxx-xxx:~$ 
```

For more information, please refer to AWS user guide - [Accessing Instances Linux](
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html).