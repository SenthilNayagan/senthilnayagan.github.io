---
layout: post
title:  "Introduction to Apache YARN"
kicker: "Resource Management"
subtitle: "???."
--image: assets/images/posts-cover-images/inverted-index.jpg
author: senthil
date: 2023-01-01 00:00:00 +0530
tags: [ "resource-management", "apache-yarn", "distributed-system" ]
categories: apache-yarn
featured: false
hidden: true
toc: true
---

# Introduction to Apache YARN

The term "YARN" stands for "**Y**et **A**nother **R**esource **N**egotiator." As the name implies, Apache YARN is a resource manager designed to *separate* the functions of *resource management* and *job scheduling/monitoring* into separate daemons. In other words, YARN separates resource management and processing engine. This not only improves Hadoop, but also makes YARN a standalone component that can be used with other data processing engines like Apache Spark.

Hadoop 2.0 added Apache YARN to its ecosystem to provide a platform for processing data that doesn't just use MapReduce but can also use other data processing engines. The YARN infrastructure is responsible for providing computational resources such as CPUs and memory that are required to run the various applications. The YARN infrastructure and HDFS are fundamentally separate entities. YARN provides resources for running an application, while HDFS provides storage.

> **Backward compatibility with Hadoop 1.x:** MapReduce in Hadoop 2 maintains API compatibility with previous stable release (Hadoop 1.x). This means that all MapReduce jobs should still run unchanged on top of YARN with just a recompile.

# YARN architecture

There are three major components to the YARN architecture:

- **Resource Manager (RM)**
- **Node Manager (NM)**
- **ApplicationMaster (AM)**

## Resource Manager (RM)

The ResourceManager, or RM is the master daemon of YARN, which is usually one per cluster. In other words, it's the master server. RM is responsible for managing the global assignments of resources (CPU and memory) among all the applications. The DataNode's location and available resources are both known (referred as rack awareness) to the RM.

ResourceManager has two main components:

- **Scheduler**
- **ApplicationsManager**

### Scheduler

The Scheduler is responsible for allocating resources to the various running applications, using the standard limitations of capacity, queues, etc. Since it is exclusively dealing with task scheduling, it does not perform any tracking or monitoring of applications. Furthermore, it offers no guarantees about restarting failed tasks either due to application failure or hardware failures. The Scheduler performs its scheduling function based on the resource requirements of the applications.

The Scheduler has a pluggable policy which is responsible for partitioning the cluster resources among the various queues, applications etc. The current schedulers such as the **CapacityScheduler** and the **FairScheduler** would be some examples of plug-ins.

### ApplicationsManager

The ApplicationsManager is responsible for accepting job submissions, negotiating the first container for executing the application-specific **ApplicationMaster**, and providing the service for restarting the ApplicationMaster container on failure.

## ApplicationMaster (AM)

Every application has a specific ApplicationMaster associated with it, i.e., one AM is assigned to each application. AM negotiates resources with the Resource Manager and works with the Node Manager. Specifically, the ApplicationMaster has the responsibility of negotiating appropriate resource containers from the Scheduler, tracking their status, and monitoring progress.

