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

# What is fault tolerance in Spark RDD?

Fault refers to failure or defect or flaw. Fault tolerance is the ability of a system to continue working normally in the event of the failure of one or more of its components.

RDDs have the capability of automatically recovering from failures. Traditionally, distributed computing systems have provided fault tolerance through *data replication* or *checkpointing*. However, Spark uses a different approach called "lineage." In data-intensive applications, lineage-based recovery is much more efficient than replication. It saves both time (since writing data over the network takes significantly longer than writing it to RAM) and memory space.

The operations carried out in an RDD are a set of Scala functions that are run on that partition of the RDD. This set of operations is combined to form a DAG. RDD tracks the graph of transformations (in DAG) that was used to build it and reruns these operations on base data to reconstruct any lost partitions.