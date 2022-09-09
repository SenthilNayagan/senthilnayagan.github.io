---
layout: post
title:  "Kubernetes Basics"
kicker: "Kubernetes"
subtitle: "Kubernetes is a Google-developed open source container orchestration platform for managing microservices or containerized applications across a distributed cluster of nodes."
image: assets/images/posts-cover-images/kubernetes-basics.jpg
author: senthil
date: 2022-08-17 00:00:00 +0530
tags: ["kubernetes", "container-orchestration", "container-management"]
categories: kubernetes
featured: false
hidden: true
---

> **Writing in progress:** If you have any suggestions for improving the content or notice any inaccuracies, please email me at [hello@senthilnayagan.com](mailto:hello@senthilnayagan.com). Thanks!

# What is Kubernetes?
Kubernetes is a Google-developed open source *container orchestration platform* for *creating*, *deploying*, and *managing* microservices or containerized applications (applications running in containers) across a distributed cluster of nodes. 

## Shared libraries
These applications come in many different shapes and sizes. They begin by taking input, manipulating the data, and then providing the results. Application programs normally have a language runtime, libraries, and our source code as their primary components. Our program will, in many instances, make use of external shared libraries. These external libraries are generally shipped as *shared components*, aka *shared objects*, in the OS that we have installed on a particular machine.

When using more traditional methods to execute multiple applications on a single computer, it is necessary for all of these programs to share the same version of shared libraries that are installed on the system. If the various applications are based on different versions of shared libraries, then the common dependencies between these groups will add an unnecessary layer of complexity.

When developers want to deploy an application on multiple computers, they use Kubernetes rather than deploying the application on each computer individually. Kubernetes provides *an abstraction layer on top of the underlying infrastructure* (computers, networks, etc.) for both *users* and *applications*. Kubernetes hides the infrastructure underneath from the applications, making development and configuration simpler.

|![Figure 1: Kubernetes provides an abstraction on top of underlying infrastructure](/assets/images/posts/kubernetes-basics.png "Created by Author")|
|:-:|
|<sup>*Figure 1: Kubernetes provides an abstraction on top of underlying infrastructure.*</sup>|<br/><br/>

Kubernetes is highly resilient, with zero downtime, rollback, scalability, and self-healing of containers. Kubernetes' primary goal is to hide the complexity of managing a collection of containers.