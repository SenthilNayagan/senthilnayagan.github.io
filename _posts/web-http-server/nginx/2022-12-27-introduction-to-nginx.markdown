---
layout: post
title:  "Introduction to NGINX"
kicker: "Web Server"
subtitle: "???."
--image: assets/images/posts-cover-images/inverted-index.jpg
author: senthil
date: 2022-12-27 00:00:00 +0530
tags: [ "nginx", "web-server" ]
categories: web-server
featured: false
hidden: true
toc: false
---

# Overview

Nginx (pronounced "engine x") is a web server that can also be used as:

- Reverse proxy
- Load balancer
- Mail proxy
- HTTP cache

## Installation

We can confirm that NGINX is running by using the following command:

```bash
ps -ef | grep nginx
  501 92245     1   0  9:44AM ??         0:00.00 nginx: master process nginx
  501 92246 92245   0  9:44AM ??         0:00.00 nginx: worker process
```

If NGINX is running, we will always see a master and one or more worker processes as shown above. Keep in mind that the master process is running as root, as NGINX requires elevated privileges in order to function properly. NGINX must be running as daemon in order for it to serve requests. We can check if NGINX is running as a daemon or in the foreground by using the `ps` command.