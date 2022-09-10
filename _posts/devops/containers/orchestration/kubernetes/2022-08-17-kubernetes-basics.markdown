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
toc: true
---

> **Writing in progress:** If you have any suggestions for improving the content or notice any inaccuracies, please email me at [hello@senthilnayagan.com](mailto:hello@senthilnayagan.com). Thanks!

# What is Kubernetes?
Kubernetes is a Google-developed open source **container orchestration platform** for **automating** the *deployment*, *scaling*, and *management of containerized applications* across a distributed cluster of nodes. Containerized applications are those that run *inside containers*. 

The name Kubernetes originates from Greek, meaning helmsman or pilot. Kubernetes is often shortened to **K8s**, which comes from the fact that there are eight letters between the "K" and the "s." Kubernetes has become the de facto standard for container orchestration.

Containers are a good way to bundle and run our applications.  In a production environment, it's important to keep an eye on and manage the containers that run the applications to make sure there's no downtime. In the event that one container goes down, another needs to be started. If a system were responsible for managing this behavior, wouldn't it make things simpler?

That's how Kubernetes comes to the rescue! Kubernetes provides us a framework that allows us to run distributed systems in a resilient manner. It takes care of scaling and failover for our application.

Kubernetes provides us with:

- **Service discovery**: Kubernetes has the ability to expose a container by using either the DNS name or their own IP address.
- **Load balancing**: Kubernetes is able to load balance and distribute the network traffic to ensure that the deployment remains stable even when there is a huge volume of traffic to a container.
- **Storage orchestration**: Kubernetes allows us to automatically mount a storage system of our choice.
- **Automated rollouts and rollbacks**: Using Kubernetes, we are able to specify the desired state of our deployed containers, and it is able to transform the actual state to the desired state at a controlled rate. For instance, we might automate Kubernetes to create new containers for our deployment, delete any existing containers, and transfer all of those containers' resources to the newly generated container.
- **Automatic bin packing**: We provide a cluster of nodes available to Kubernetes so that it may utilize those nodes to run containerized tasks. We inform Kubernetes of the required amount of memory and processor for each container. Kubernetes enables us to make the most efficient use of our resources by fitting containers onto our nodes.
- **Self-healing**: Kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't expose them to clients until they are ready to serve.
- **Secret and configuration management**: We are able to store and handle sensitive information with the help of Kubernetes. This includes passwords, OAuth tokens, and SSH keys. We can deploy and update secrets and application configuration without rebuilding our container images, and without exposing secrets in our stack configuration.

## Packing applications in containers
These applications come in many different shapes and sizes. They begin by taking input, manipulating the data, and then providing the results. Application programs normally have a *language runtime*, *libraries* including external libraries, *configurations*, and *source code* as their primary components.

There are many ways to deploy these applications as a whole:

- **Configuration management systems:** We can use configuration management systems like Puppet or Ansible, which use code to install, run, configure, and update applications.
- **Omnibus package:** It's an effort to include everything that the program requires in a single file as much as possible. For example, a Java omnibus package would include the Java runtime as well as all the JAR files for the application.

From an operational point of view, we need to be able to handle not just all of these various kinds of packages or applications, but also a fleet of servers on which to host them. These servers need to be provisioned, networked, deployed, configured, kept up-to-date with security patches, monitored, and managed. This all takes a significant amount of time, technical expertise, and effort just to provide a platform to run applications on.

# Kubernetes components

Kubernetes cluster is made up of:

- A **control plane** (aka **master**).
- A set of **worker machines** known as **worker nodes** or simply **workers**.
- A **distributed storage system** that is responsible for maintaining the cluster's consistent state.

When we deploy Kubernetes, it creates a **cluster**. A Kubernetes cluster is made up of a set of machines known as **nodes**. The nodes can be virtual machines (VMs) or physical machines. The nodes include a **master node** and a set of **worker nodes**. The worker nodes are responsible for running containerized applications. At least one worker is present in every cluster. Our applications that are contained inside the containers. Each worker node holds one or more containers.

The worker node(s) hosts the **Pods**. A pod represents one or more running containers. The **control plane** is responsible for managing the cluster's worker nodes as well as the Pods. The control plane generally runs across multiple nodes in production environments, which provides fault tolerance and high availability.

## What is a control plane and why do we need it?

The control plane is the central nervous system of a Kubernetes cluster. It ensures that every component in the cluster is kept in the *desired state*. Desired state is one of the core concepts of Kubernetes. It's the state that we want the system to be in. The actual state, on the other hand, is the state that the system is actually in.

Control plane receives data about *internal cluster events*, *external systems*, and *third-party applications*. It analyses the data, and based on that, it takes decisions and puts them into action. The control plane manages and maintains the worker nodes that hold the containerized applications.

### Components of the control plane

The control plane is made up of several components. All these components run on a node called the primary node or **master node**.

- An **API server** as **kube-apiserver**
- A **scheduler** as **kube-scheduler**
- A **controller manager** as **kube-controller-manager**
- A **persistent data store** as **etcd**
- A **cloud controller manager** as **cloud-controller-manager**

# TODO
When developers want to deploy an application on multiple computers, they use Kubernetes rather than deploying the application on each computer individually. Kubernetes provides *an abstraction layer on top of the underlying infrastructure* (computers, networks, etc.) for both *users* and *applications*. Kubernetes hides the infrastructure underneath from the applications, making development and configuration simpler.

|![Figure 1: Kubernetes provides an abstraction on top of underlying infrastructure](/assets/images/posts/kubernetes-basics.png "Created by Author")|
|:-:|
|<sup>*Figure 1: Kubernetes provides an abstraction on top of underlying infrastructure.*</sup>|<br/><br/>

Kubernetes is highly resilient, with zero downtime, rollback, scalability, and self-healing of containers. Kubernetes' primary goal is to hide the complexity of managing a collection of containers.

## Kubernetes installation

### Installation of Kubernetes command-line tools

The command-line tools for Kubernetes are as follows:

- `kubectl`
- `kind`
- `minikube`
- `kubeadm`

#### kubectl

The Kubernetes command-line tool is known as `kubectl`. It allows us to run commands against Kubernetes clusters. We can use `kubectl` to: 

- Deploy applications
- Inspect and manage cluster resources
- View logs

#### kind

`kind` lets us run Kubernetes on our local computer. This tool requires that we have Docker installed and configured.

#### minikube

Like `kind`, `minikube` is a tool that enables us to run Kubernetes on a local computer. In order for us to get a feel for Kubernetes, `minikube` runs a Kubernetes cluster with a *single node* on each of our individual computers (including Windows, macOS, and Linux PCs).

To install the latest `minikube` stable release on on x86-64 macOS using Homebrew:

```bash
brew install minikube
```

#### kubeadm

We can use the `kubeadm` tool to create and manage Kubernetes clusters. It performs the tasks required to get a minimum viable, secure cluster up and running in a manner that is friendly to users.

### Kubernetes configuration

# Frequently asked questions (FAQ)

## What is the workload in Kubernetes?

A workload is an *application* running on Kubernetes. On Kubernetes, our workload is always run within a collection of pods, regardless of whether it consists of a single component or several that work together.