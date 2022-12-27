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

# Installation

We can confirm that NGINX is running by using the following command:

```bash
ps -ef | grep nginx
  501 92245     1   0  9:44AM ??         0:00.00 nginx: master process nginx
  501 92246 92245   0  9:44AM ??         0:00.00 nginx: worker process
```

If NGINX is running, we will always see a master and one or more worker processes as shown above. Keep in mind that the master process is running as root, as NGINX requires elevated privileges in order to function properly. NGINX must be running as daemon in order for it to serve requests. We can check if NGINX is running as a daemon or in the foreground by using the `ps` command.

# NGINX files and directories

## /etc/nginx/nginx.conf

The `/etc/nginx/nginx.conf` file is the default configuration entry point used by the NGINX service.

## /etc/nginx/conf.d/

The `/etc/nginx/conf.d/` directory contains the default HTTP server configuration file.

# NGINX terminologies

## Directives

Directives are key-value pairs found in the `ngix.conf` file. For example, `worker_processes 1;` is one of the directives in the config file.

## Context

The block starting with open and closing curly braces is called a context. Below is the context. Within the context, we can have directives for that specific context. 

```text
server {
    listen       8080;
    server_name  localhost;
}
```


