---
layout: post
title:  "Stateful vs. Stateless Applications"
kicker: "Web Server"
subtitle: "???."
--image: assets/images/posts-cover-images/inverted-index.jpg
author: senthil
date: 2022-12-27 00:00:00 +0530
tags: [ "web-server" ]
categories: web-server
featured: false
hidden: true
toc: false
---

# A breif ovewrive of network protocol

A network protocol is a *set of rules* that dictates how different devices on the same network format, send, and receive data, no matter how different their underlying infrastructures are. Devices on both ends of a communication exchange must accept and adhere to protocol norms for data to be sent and received effectively.

Network protocols can be broadly classified into two types:

- **Stateful**
- **Stateless**

The term "state" refers to the condition or quality of being at a given point in time. Whether a protocol is stateful or stateless depends on how long a client stays in contact with it and how much of the information is stored.

## What is stateless?

A stateless architecture or application is a type of network protocol in which the state of previous transactions is not stored or referenced in later (or subsequent) transactions. Every transaction is treated as if it were the first time it had ever occurred.

In a stateless protocol, it is not the receiver's job to remember any session state from requests that came before. The sender sends all the necessary session state to the receiver in a way that lets each request be handled independently from the session data associated with the requests that came before it.

Take the act of sending a text message as an example. In this case, the availability of the recipient is not verified before the SMS is sent. The receiving device does not acknowledge to the transmitting device that it has received the message. It's possible but not guaranteed that the recipient will really get the message even if it was sent. There has to be no double-checking of status or retries. This is what it means to be stateless.

**HTTP** (Hypertext Transfer Protocol) is an example of a stateless protocol because each request is processed independently of the requests that came before it. This means that the browser and the server will lose contact once a transaction has been completed.

### Advantages of stateless

TODO

### Disadvantages of stateless

TODO

## What is stateful?

TODO

### Advantages of stateful

TODO

### Disadvantages of stateful

TODO

# Conclusions

TODO
