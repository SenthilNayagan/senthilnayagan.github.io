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

When developers want to deploy an application on multiple computers, they use Kubernetes rather than deploying the application on each computer individually. Kubernetes provides *an abstraction layer on top of the underlying infrastructure* (computers, networks, etc.) for both *users* and *applications*. Kubernetes hides the infrastructure underneath from the applications, making development and configuration simpler. Kubernetes' primary goal is to hide the complexity of managing a collection of containers.

|![Figure 1: Kubernetes provides an abstraction on top of underlying infrastructure](/assets/images/posts/kubernetes-basics.png "Created by Author")|
|:-:|
|<sup>*Figure 1: Kubernetes provides an abstraction on top of underlying infrastructure.*</sup>|<br/><br/>

Kubernetes provides us with:

- **Service discovery**: Kubernetes has the ability to expose a container by using either the DNS name or their own IP address.
- **Load balancing**: Kubernetes is able to load balance and distribute the network traffic to ensure that the deployment remains stable even when there is a huge volume of traffic to a container.
- **Storage orchestration**: Kubernetes allows us to automatically mount a storage system of our choice.
- **Automated rollouts and rollbacks**: Using Kubernetes, we are able to specify the desired state of our deployed containers, and it is able to transform the actual state to the desired state at a controlled rate. For instance, we might automate Kubernetes to create new containers for our deployment, delete any existing containers, and transfer all of those containers' resources to the newly generated container.
- **Automatic bin packing**: We provide a cluster of nodes available to Kubernetes so that it may utilize those nodes to run containerized tasks. We inform Kubernetes of the required amount of memory and processor for each container. Kubernetes enables us to make the most efficient use of our resources by fitting containers onto our nodes.
- **Self-healing**: Kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't expose them to clients until they are ready to serve.
- **Secret and configuration management**: We are able to store and handle sensitive information with the help of Kubernetes. This includes passwords, OAuth tokens, and SSH keys. We can deploy and update secrets and application configuration without rebuilding our container images, and without exposing secrets in our stack configuration.

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

#### kube-apiserver

The core of Kubernetes' control plane is the API server called Kubernetes API. Kubernetes API acts as the *interface* via which the control plane communicates with the worker nodes and other external systems. An API can be anything that’s used for programmatic communication. The Kubernetes API in particular is RESTful. The command line interface, web user interface, users, and services communicate with the cluster through Kubernetes API.

The Kubernetes API lets us query and manipulate the state of API objects in Kubernetes such as Pods, Namespaces, ConfigMaps, and Events. The main implementation of the Kubernetes API server is the `kube-apiserver`. Since it is meant to extend horizontally, we are able to deploy many instances of the `kube-apiserver` in order to evenly distribute the load.

#### Persistent data store (etcd)

Persistent data storage is provided by [**etcd**](https://etcd.io/){:target="_blank"}, which is a distributed, reliable key-value store. It is a standalone open source tool, and Kubernetes communicates to it using the `kube-apiserver`.

It stores the configuration information that is needed by the worker nodes as well as other data that is required to manage the cluster.

# Kubernetes command-line tools

The command-line tools for Kubernetes are as follows:

- `kubectl`
- `kind`
- `minikube`
- `kubeadm`

## kubectl

The Kubernetes command-line tool is known as `kubectl`. It allows us to run commands against Kubernetes clusters. We can use `kubectl` to: 

- Deploy applications
- Inspect and manage cluster resources
- View logs

To verify that your cluster is working, use the following command:

```bash
$ kubectl cluster-info

Kubernetes control plane is running at https://127.0.0.1:50918
CoreDNS is running at https://127.0.0.1:50918/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

The above output of `kubectl cluster-info` command indicates that the control plane is active and responding to requests.

Now use the `kubectl` command to list all nodes in our cluster:

```bash
$ kubectl get nodes

NAME       STATUS   ROLES           AGE    VERSION
minikube   Ready    control-plane   7h8m   v1.24.3
```

The above output shows a cluster with single node.

> **Note:** Everything in Kubernetes is represented as an object, which can be obtained and manipulated via the RESTful API. The `kubectl get` command retrieves a list of objects of the specified  type from the API. In the above case, it's of type "node".

To see more detailed information about an object, we use the following command:

```bash
$ kubectl describe node <name of the node>
```

Because it is such a large output, I have chosen to exclude the actual output that the `describe` command generates.

## kind

`kind` lets us run Kubernetes on our local computer. This tool requires that we have Docker installed and configured.

## minikube

Like `kind`, `minikube` is a tool that enables us to run Kubernetes on a local computer. In order for us to get a feel for Kubernetes, `minikube` runs a Kubernetes cluster with a *single node* on each of our individual computers (including Windows, macOS, and Linux PCs).

To install the latest `minikube` stable release on on x86-64 macOS using Homebrew:

```bash
$ brew install minikube
```

### Start our cluster

To start a single-node cluster:

```bash
$ minikube start
```

### minikube dashboard

minikube has integrated support for the web-based dashboard. To access the web-based dashboard:

```bash
$ minikube dashboard
```

This will enable the dashboard add-on, and open the proxy in the default web browser. If we we do not want to launch a web browser, we can instruct the dashboard command to instead merely output a URL as shown below:

```bash
$ minikube dashboard --url
```

We can use dashboard to:

- Deploy containerized applications to a Kubernetes cluster.
- Troubleshoot your containerized application.
- Manage the cluster resources.
- Get an overview of applications running on your cluster.
- Creating or modifying individual Kubernetes resources such as Deployments, Jobs, DaemonSets, etc.

## kubeadm

We can use the `kubeadm` tool to create and manage Kubernetes clusters. It performs the tasks required to get a minimum viable, secure cluster up and running in a manner that is friendly to users.

# Deploying a Kubernetes cluster

There are many different ways that Kubernetes cluster can be deployed:

- Installing and configuring a full-fledged Kubernetes cluster with multiple nodes.
- Using the built-in Kubernetes cluster in Docker Desktop.
- Running a local cluster using minikube CLI

## Installing and configuring a full-fledged Kubernetes cluster with multiple nodes

It is not an easy job to set up a full-fledged Kubernetes cluster with multiple nodes, particularly if we are not familiar with Linux or network management. A proper Kubernetes installation involves the use of many physical or virtual machines and requires proper network setup to allow all containers in the cluster to communicate with one another.

A full-ledged Kubernetes cluster can be installed:

- On our our **local machines**.
- On **virtual machines** provided by cloud providers like Google Compute Engine, Amazon EC2, Microsoft Azure, and so on.
- On the other hand, almost all cloud service providers now offer **managed Kubernetes services**, which saves us the trouble of installing and managing it. A brief list of the services offered by the most popular cloud providers follows: Google offers **GKE**, Amazon has **EKS**, Microsoft has **AKS**, and so on.

Keep in mind that installing and managing Kubernetes ourselves is a much more complicated task than just using it, especially before we know a lot about its architecture and how it works.

## Using the built-in Kubernetes cluster in Docker Desktop

Docker Desktop for macOS and Windows comes preinstalled with a single-node Kubernetes cluster that can be enabled by going into the Settings dialog box as shown in *Figure 2* below.

|![Figure 2: Kubernetes preinstalled with Docker Desktop](/assets/images/posts/kurbernetes-comes-with-docker-desktop.png)|
|:-:|
|<sup>*Figure 2: Kubernetes preinstalled with Docker Desktop.*</sup>|<br/><br/>

## Running a local cluster using minikube CLI

`minikube`, which is a command-line tool that is maintained by the Kubernetes community, is yet another method that can be used to create a Kubernetes cluster. It enables us to run Kubernetes on a local computer. In order for us to get a feel for Kubernetes, minikube runs a Kubernetes cluster with a single node on each of our individual computers (including Windows, macOS, and Linux PCs).

### Installing minikube CLI

minikube CLI supports macOS, Linux, and Windows. It just consists of a single executable binary file, which can be found in the minikube repository on [GitHub](https://github.com/kubernetes/minikube){:target="_blank"}. More information on how to install it for various operating systems can be found [here](https://minikube.sigs.k8s.io/docs/start/){:target="_blank"}.  

## Packing applications in containers
These applications come in many different shapes and sizes. They begin by taking input, manipulating the data, and then providing the results. Application programs normally have a *language runtime*, *libraries* including external libraries, *configurations*, and *source code* as their primary components.

There are many ways to deploy these applications as a whole:

- **Configuration management systems:** We can use configuration management systems like Puppet or Ansible, which use code to install, run, configure, and update applications.
- **Omnibus package:** It's an effort to include everything that the program requires in a single file as much as possible. For example, a Java omnibus package would include the Java runtime as well as all the JAR files for the application.

From an operational point of view, we need to be able to handle not just all of these various kinds of packages or applications, but also a fleet of servers on which to host them. These servers need to be provisioned, networked, deployed, configured, kept up-to-date with security patches, monitored, and managed. This all takes a significant amount of time, technical expertise, and effort just to provide a platform to run applications on.

### Kubernetes configuration

# Frequently asked questions (FAQ)

## What is the workload in Kubernetes?

A workload is an *application* running on Kubernetes. On Kubernetes, our workload is always run within a collection of pods, regardless of whether it consists of a single component or several that work together.