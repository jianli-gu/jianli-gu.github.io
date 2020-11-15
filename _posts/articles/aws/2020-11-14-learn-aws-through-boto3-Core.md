---
layout: post
title: Learn AWS through boto3 - Core
categories: [AWS]
tags: [AWS, Python, Boto3]
modified: 2020-11-14
---


## Boto3

What consists of `boto3`? Namely,

- Session
- Config
- Resources 
- Clients

![boto3-and-botocore](images/2020-11-14-boto3-and-botocore.png)
*source: https://www.youtube.com/watch?v=Cb2czfCV4Dg&ab_channel=AmazonWebServices*


## Session

`boto3` session stores AWS configurations, like credentials and region name, and can create clients 
and resources. It creates default session if you don't create one explicitly.


## Config

`boto3` configures client-specific configurations that affect the behavior of your specific client object.

## Resources

It provides object-oriented API for AWS for high-level service access recommended.

## Clients

It exposes `botocore` clients to developers for low-level service access.