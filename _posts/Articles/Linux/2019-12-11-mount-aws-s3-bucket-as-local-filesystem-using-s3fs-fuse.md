---
layout: post
title: Mount AWS S3 Bucket as Local Filesystem Using s3fs-fuse
categories: [Linux]
tags: [AWS, S3]
modified: 2019-12-11
---

[FUSE](https://github.com/libfuse/libfuse) (Filesystem in Userspace) is an interface 
for userspace programs to export a filesystem to the Linux kernel. [S3FS-FUSE](https://github.com/s3fs-fuse/s3fs-fuse) 
is a FUSE filesystem that allows users to mount an AWS S3 bucket as a local filesystem 
that stores and extracts files natively and transparently in S3. It's stable and has been 
using in many production environments.

How to set it up? Please follow the steps below.

###### Step 1:
Create an AWS account, and create a S3 bucket.

###### Step 2:
Install `s3fs-fuse`, please refer to [S3FS-FUSE README](https://github.com/s3fs-fuse/s3fs-fuse#installation)

###### Step 3:
Setup security credentials using one of the following approaches.
* Using the passwd_file command line option
* Setting the `AWSACCESSKEYID` and `AWSSECRETACCESSKEY` environment variables
* Using a `.passwd-s3fs` file in your home directory
* Using the system-wide `/etc/passwd-s3fs` file

###### Step 4:
Mount S3 bucket
```bash
s3fs mybucket /mnt
```
For more details, please refer to [https://github.com/s3fs-fuse/s3fs-fuse/wiki/Fuse-Over-Amazon](
https://github.com/s3fs-fuse/s3fs-fuse/wiki/Fuse-Over-Amazon)


##### Limitations
However, there are also some limitations. More specifically:

* Read and write performance can not match the local filesystem.
* Random writes or appends to files require rewriting the entire file.
* Eventual consistency can temporarily yield stale data.
* No atomic renames of files or directories.

##### One more thing
`s3fs-fuse` can also setup in docker environment!
