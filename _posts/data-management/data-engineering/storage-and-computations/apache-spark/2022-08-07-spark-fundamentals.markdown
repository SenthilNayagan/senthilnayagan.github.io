---
layout: post
title:  "Apache Spark Fundamentals"
kicker: "Apache Spark"
subtitle: "Apache Spark is a unified computing engine for distributed data processing and it has become the de facto tool for any developer or data scientist interested in big data."
image: assets/images/posts-cover-images/spark-fundamentals.jpg
author: senthil
published_on: 2022-08-07 00:00:00 +0530
tags: ["apache-spark"]
categories: apache-spark
featured: false
hidden: true
---

The necessity for data distribution over several computers arises from either the data being too massive to store on a single machine or the computation being too long to accomplish on a single machine.

# Spark architecture

Spark is built on a *master-slave architecture*, which we refer to as the *Spark cluster*. Spark cluster is made up of one master node and one or more worker nodes. Each worker node has at least one *executor*.

|![Spark Architecture](/assets/images/posts/spark-architecture.png)|
|:-:|
|<sub><sup>*Apache Spark Architecture.*</sup></sub>|<br/><br/>

> **Note:** From Spark 2.0 onwards we can access SparkContext object through SparkSession.

## Spark driver
The Spark driver is a Java process that runs the main() function of the user program. In other words, the user program’s main() function executes in the driver process. It runs on the master node of the Spark cluster. It is the core of a Spark Application and stores all relevant information during the application's lifetime. When we’re working with a Spark shell, the driver is part of the shell.

The driver is responsible for:
- Maintaining information about the Spark application.
- Responds to user's program or input.
- Analyzing, distributing, and scheduling work (as tasks) across executors.

## Executor

An executor resides in the worker node, and each worker node consists of one or more executors. Executors are responsible for running one or more *tasks*. Executors are launched at the start of a Spark application in coordination with the cluster manager. The driver launches and removes executors dynamically as needed.

Each executor is responsible for:
- Executing tasks assigned to it by the driver and reporting the status of the computation to the driver.
- Caching (in memory) or persisting (on disk) the data in the worker node.

## Cluster manager

The cluster manager controls physical machines and allocates resources such as CPU, memory, and so on to Spark applications.

### Cluster manager types

Apache Spark currently supports the following cluster managers:
- Standalone - a basic cluster manager included with Spark that makes it simple to build up a cluster.
- Hadoop YARN - the resource manager in Hadoop 2 and 3.
- Kubernetes - an open-source system for automating deployment, scaling, and management of containerized applications.

#### Local mode

Spark features a *local* mode in addition to its cluster mode. In local mode, the driver and executors run as threads on our own machine rather than as part of a cluster.

# Spark's APIs

Spark has two core sets of APIs:
- Low-level APIs
- High-level APIs

# What is fault tolerance in Spark RDD?

Fault refers to failure or defect or flaw. Fault tolerance is the ability of a system to continue working normally in the event of the failure of one or more of its components.

RDDs have the capability of automatically recovering from failures. Traditionally, distributed computing systems have provided fault tolerance through *data replication* or *checkpointing*. However, Spark uses a different approach called "lineage." In data-intensive applications, lineage-based recovery is much more efficient than replication. It saves both time (since writing data over the network takes significantly longer than writing it to RAM) and memory space.

The operations carried out in an RDD are a set of Scala functions that are run on that partition of the RDD. This set of operations is combined to form a DAG. RDD tracks the graph of transformations (in DAG) that was used to build it and reruns these operations on base data to reconstruct any lost partitions.

