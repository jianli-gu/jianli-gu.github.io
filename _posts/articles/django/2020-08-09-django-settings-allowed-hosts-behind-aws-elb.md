---
layout: post
title: Django Settings ALLOWED_HOSTS behind AWS ELB
categories: [Django]
tags: [Django, AWS, ELB, ECS]
modified: 2020-08-09
---

`ALLOWED_HOSTS` is an important setting for security when deploying the Django application in production
environment. It's a list of strings representing the host/domain names that the Django site can serve.

Djnago application deployed on AWS ECS is behind AWS ELB which distributes incoming application 
traffic across multiple targets, like EC2 instances, containers and IP addresses. However, 
the `ALLOWED_HOSTS` settings blocks internal requests from AWS ELB, because ELB health check uses 
the internal ip address instead of a domain name. We can simply set `ALLOWED_HOST=['*']`, but it's 
not a good solution from the perspective of security.

Here's an alternative solution, it can read the `ec2metadata` of EC2 instance to get the internal IP, 
and add it into Django's `ALLOWED_HOSTS` list dynamically. 
To add the following code into the `settings.py` file,


```python
import requests

ALLOWED_HOSTS = [
    'yourdomain.tld',
    '.compute-1.amazonaws.com', # allows viewing of instances directly
]

EC2_PRIVATE_IP = None
try:
    EC2_PRIVATE_IP = requests.get('http://169.254.169.254/latest/meta-data/local-ipv4', timeout = 0.1).text
except requests.exceptions.RequestException:
    pass

if EC2_PRIVATE_IP:
    ALLOWED_HOSTS.append(EC2_PRIVATE_IP)
```

Hope it can help, good luck!

---
**References**

[1] [https://gist.github.com/dryan/8271687](https://gist.github.com/dryan/8271687)
