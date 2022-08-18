---
layout: post
title:  "Kubernetes Basics"
kicker: "Kubernetes"
subtitle: "Kubernetes is a Google-developed open source container orchestration platform for managing microservices or containerized applications across a distributed cluster of nodes."
image: assets/images/posts-cover-images/kubernetes-basics.jpg
author: senthil
published_on: 2022-08-17 00:00:00 +0530
tags: ["kubernetes", "container-orchestration", "container-management"]
categories: kubernetes
featured: false
hidden: true
---

# What is Kubernetes?
Kubernetes is a Google-developed open source *container orchestration platform* for managing microservices or containerized applications (applications running in containers) across a distributed cluster of nodes. 

When developers want to deploy an application on multiple computers, they use Kubernetes rather than deploying the application on each computer individually. Kubernetes provides *an abstraction layer on top of the underlying infrastructure* (computers, networks, etc.) for both consumers and applications. Kubernetes hides the infrastructure underneath from the applications, making development and configuration simpler.

|![Figure 1: Kubernetes provides an abstraction on top of underlying infrastructure](/assets/images/posts/kubernetes-basics.png)|
|:-:|
|<sup>*Figure 1: Kubernetes provides an abstraction on top of underlying infrastructure.*</sup>|<br/><br/>

Kubernetes is highly resilient, with zero downtime, rollback, scalability, and self-healing of containers. Kubernetes' primary goal is to hide the complexity of managing a collection of containers.