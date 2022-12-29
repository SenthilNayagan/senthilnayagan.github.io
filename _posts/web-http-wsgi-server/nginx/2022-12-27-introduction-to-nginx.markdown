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
toc: true
---

# Introduction to NGINX

Nginx (pronounced "engine x") is free and open source software that may serve as:

- Web/HTTP server
- Reverse proxy
- Load balancer
- Mail proxy
- HTTP cache

The initial goal was to develop a fast and reliable web server, in particular to address the **C10K** problem. The C10k problem is the inability of a server to handle a high number of concurrent connections (the 10K) before running out of available resources.

> **Web server vs. HTTP server:** Web servers and HTTP servers are almost synonymous, with just a few minor distinctions. The term "web server" is more generic or broad. The term "HTTP server" often refers to a software implementation of the server part of the protocol (HTTP protocol). A web server typically uses more than one protocol, and HTTP is one of them. To communicate with clients such as web browsers, the HTTP server employs the HTTP protocol. Since web servers use the HTTP protocol to serve clients, one could say that the HTTP server is contained within the web server.

In addition to its HTTP server capabilities, NGINX can also function as a proxy server for email (IMAP, POP3, and SMTP) and a reverse proxy and load balancer for HTTP, TCP, and UDP servers.

## NGINX as a web server

As a web server, NGINX's job is to serve static or dynamic content to the clients. A primary motivation for developing NGINX was the need to create a web server that could handle massive amounts of traffic at lightning speeds, and this remains a primary focus. When comparing web server performance, NGINX consistently outperforms Apache and other popular servers.

## NGINX beyond web serving

Despite NGINX's initial popularity as the fastest web server, its fundamental architecture has shown to be very suitable for many web tasks beyond serving content. NGINX is often used as a reverse proxy and load balancer to control incoming traffic and transfer it to slower upstream servers due to its high connection throughput.

NGINX also is frequently placed between clients and a second web server, to serve as an SSL/TLS terminator or web accelerator. NGINX acts as an intermediate, handling activities like SSL/TLS negotiation and content compression and caching that may otherwise slow down our web server. Dynamic sites commonly deploy NGINX as a content cache and reverse proxy to reduce load on application servers.

---

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

---

# NGINX terminologies

## Directives

Directives are key-value pairs found in the `ngix.conf` file. For example, `worker_processes 1;` is one of the directives in the config file. Technically, everything inside a NGINX configuration file is a directive.

Directives are of two types:

- **Simple directives** - Simple directives are the ones terminated by semicolons.
- **Block directives** - However, unlike simple directives, which end with semicolons, block directives end with a pair of curly brackets enclosing additional instructions.


## Context

A block directive capable of containing other directives inside it is called a context. There are four core contexts in NGINX:

- `events { }` - NGINX's global configuration for handling requests is set in the `events` context. In a valid configuration file, there should be only one `events` context.
- `http { }` - As the name suggests, the `http` context is used to provide the server's configuration for processing HTTP and HTTPS requests. In a valid configuration file, there should be only one `http` context.
- `server { }` - The `server` context is nested inside the `http` context and used for configuring specific virtual servers within a single host. There can be multiple `server` contexts in a valid configuration file nested inside the `http` context. Each `server` context is considered a virtual host.
- `main` - The main context is the configuration file itself. Whatever we write that doesn't fit into one of the three previously mentioned contexts is in the main context.

Below is the context. Within the context, we can have directives for that specific context. 

```text
server {
    listen       8080;
    server_name  localhost;
}
```

## Modules

As an open source project, NGINX benefits from contributions of new features and functionality from a large developer community. These new features developed by the community are available as modules that can be dynamically plugged into a running NGINX instance. NGINX keeps a repository of third-party modules that have been tested and certified to work well with NGINX.

### Upstream module

Upstream is a module used in NGINX to define the servers to be load balanced. Each server can listen on different port.

Given that we have two servers, `server1` with the IP `172.42.42.10` and `server2` with the IP `172.42.42.20`, the upstream name `sample.server.com` would look like this:

```text
upstream sample.server.com {
    server 172.42.42.10:8080 weight=2;
    server 172.42.42.20:8081;
}
```

In the above case, the name of the upstream is `sample.server.com`. A server's ID address and other parameters can be specified in an additional directive called `server`, that is contained inside the upstream. Default `upstream` use weighted round-robin balancing method. 

According to the above example, every three requests will be distributed as follows: Two requests go to server1 (172.42.42.10), and the other request goes to server2 (172.42.42.20). Any time a server is unable to fulfill a request, the request is sent to the next available server. This process continues until all available servers have been exhausted. If no servers responded successfully, the client will receive the result of the communication with the last server.

---

# NGINX configurtion

As a web server, NGINX's job is to serve static or dynamic content to the clients. The configuration files often control how that content will be served. NGINX's default configuration file is `nginx.conf`, located in `/usr/local/nginx/conf`, `/etc/nginx`, or `/usr/local/etc/nginx` (if installed via Homebrew on Mac). Unless we are sure of ourselves, we shouldn't edit the original nginx.conf file. If we make changes to the configuration file, NGINX will need to be instructed to reload the file again. We may do this in two different ways:

- We can restart the NGINX service by executing the `sudo systemctl restart nginx` command.
- We can dispatch a `reload` signal to NGINX by executing the `sudo nginx -s reload` command. The `-s` option is used for dispatching various signals (`stop`, `quit`, `reload`, `reopen`) to NGINX.


The NGINX configuration file is made up of **directives** and **contexts**. These directives and contexts control NGINX modules. Any directives placed outside of any context are considered to be in the **main context**.

---

# Frequently asked questions (FAQ)

## How to validate the NGINX configuration?

Verifying the syntax of a file is the first step after creating or editing a configuration file. This helps prevent any unforeseen errors which can cause our website to gown down. There are two different commands that may be used to do this, and they both provide the same results:

```bash
nginx -t
```

Or use one of the following:

```bash
service nginx configtest
systemctl config nginx
```

Response:

```text
nginx: the configuration file /usr/local/etc/nginx/nginx.conf syntax is ok
nginx: configuration file /usr/local/etc/nginx/nginx.conf test is successful
```