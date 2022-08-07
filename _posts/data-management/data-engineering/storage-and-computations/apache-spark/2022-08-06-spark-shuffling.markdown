---
layout: post
title:  "Shuffling in Apache Spark"
kicker: "Apache Spark"
subtitle: "Shuffling is the act of redistributing data so that it’s grouped differently across partitions. This typically involves copying data across executors and machines, making the shuffle a complex and costly operation."
image: assets/images/posts-cover-images/spark-shuffling.jpg
author: senthil
published_on: 2022-08-06 00:00:00 +0530
tags: ["apache-spark", "shuffling"]
categories: apache-spark
featured: false
hidden: true
---

# A brief introduction to Spark architecture

Spark is built on a *master-slave architecture*, which we refer to as the *Spark cluster*. Spark cluster is made up of one master node and one or more worker nodes. Each worker node has at least one *executor*. 

## Spark driver

The master node has a single central coordinator known as *driver* (aka *driver program*). The driver is a Java process that runs the main() function of the user program. In other words, the user program's main() function executes in the driver process. It creates the SparkContext (also called SparkSession) object inside the application. The driver program accesses Spark functionalities such as executors and the cluster manager through a SparkContext/SparkSession object that represents a connection to the computing cluster.

The driver is responsible for turning user code into smaller execution units known as *tasks* and scheduling them to be executed on executors with the help of a cluster manager. The driver decides the number of jobs to be performed.

> **Note:** From Spark 2.0 onwards we can access SparkContext object through SparkSession.

The driver coordinates with all the executors for the execution of tasks.

## Executor

Executor resides in the worker node, and each worker node consists of one or more executors. Executors are responsible for running one or more *tasks*. Executors are launched at the start of a Spark application in coordination with the cluster manager. The driver launches and removes executors dynamically as needed.

### Responsiblities
- To run an individual task and return the results to the driver.
- It can cache (persist, i.e., store it on the disk) the data in the worker node.

|![Spark Architecture](/assets/images/posts/spark-architecture.png)|
|:-:|
|<sub><sup>*Apache Spark Architecture.*</sup></sub>|<br/><br/>

# What is shuffling in Spark?

