---
layout: post
title:  "Partitions and Bucketing in Spark"
kicker: "Apache Spark"
subtitle: "Partitioning and bucketing are used to improve the reading of data by reducing the cost of shuffles, the need for serialization, and the amount of network traffic."
image: assets/images/posts-cover-images/spark-bucketing-and-partitions.jpg
author: senthil
published_on: 2022-07-25 15:00:00 +0530
tags: ["apache-spark", "partition", "bucketing"]
categories: apache-spark
featured: false
hidden: false
draft: true
toc: true
---

Partitioning and bucketing are used to improve the reading of data by reducing the cost of shuffles, the need for serialization, and the amount of network traffic.

# Partitioning in Spark

Apache Spark's **speed** in processing huge amounts of data is one of its primary selling points. Spark's speed comes from its ability to allow developers to run multiple tasks *in parallel* and *independently* across hundreds of machines in a cluster or across multiple cores on a desktop. It's all possible because Apache Spark **RDDs** serve as the *main interface*. These RDDs are partitioned and run in parallel behind the scenes.

Partitioning can also deliver a significant speed benefit by reducing the amount of data to be shuffled across the network.

If RDDs are too large to fit on a single node, they must be partitioned (distributed) across many nodes. Apache Spark *automatically* partitions RDDs and distributes them across different nodes. Partitions are the fundamental building blocks of parallelism in Apache Spark. We can think of "partition" as a subset of our data.

|![Data Partitioned](/assets/images/posts/apache-spark-data-partitions.png)|
|:-:|
|<sup>*Figure 1: Data Partitioned.*</sup>|<br/><br/>|

Spark is a distributed computing system, so it can operate on data partitons in parallel. A transformation or any sort of computation on a data partitons is called a **task** and each task generally takes place on one Spark core. Optimal partitioning in Spark strikes a balance between read performance and write performance.

Please take the following considerations into account:

- **Too many partitions:** Too many partitions can slow down the time it takes to read and force Spark to create more tasks to process the data, which could cause the driver to get an "out of memory" error. Overly large partitions can even cause executor "out of memory" errors.
- **Few or lack of partitions:** On the other hand, a lack of or few partitions may indicate that Spark's parallelism is not being fully utilized, resulting in long computation and write times. Furthermore, having few partitions may result in skewed data and inefficient resource use.
- **Data skewness:** We refer to data as "skewed" when it is not evenly distributed among workers. In Apache Spark, transformations like `join`, `groupBy`, and `orderBy` change data partitioning, which results in data skewness. Common effects of skewed data include the following:
    - **Slow running stages or tasks:** Certain operations will take very long to complete because of the huge volume of data that a particular worker must process.
    - **Spilling data to disk:** In the event that more data is not able to fit in the memory of a worker, it must be written to disk.
    - **Out-of-memory error:** If worker runs out of disk space, an error is thrown.

Spark generally does a good job splitting data into partitons to ensure parallelism and efficient computing. However, when dealing with very large data sets, it is sometimes necessary to manually adjust partitions to ensure optimal performance. As a rule of thumb, 128 megabytes is a good size for a data partition when dealing with data sets above 1 gigabyte. That is, **# Partitions = Dataset Size (mb) / 128 mb**.


To get us started with partitioning, here are some fundamentals:

- Every node (worker) in a Spark cluster contains one or more partitions of any size.
- By default, the number of partitions is set to the total number of cores on all the executor nodes. This is the most effective method for determining the total number of spark partitions in an RDD.
- The number of partitions in Spark is configurable.
- Only one partition is processed by one executor at a time, so the size and number of partitions handed to the executor are directly proportional to the time it takes to complete them.

## Types of partitioning

Apache Spark supports two types of partitioning:

- **Hash partitioning**
- **Range partitioning**

Partitioning decisions are influenced by a wide variety of factors, including:

- Available resources (CPU, memory, and network)
- Transformation used to derive RDD - 

## How do we get the right number of partitions?

TODO

---

# Bucketing in Spark

Bucketing is an optimisation feature that Apache Spark (also in Apache Hive) has supported since version 2.0. It's a way to improve performance by dividing data into smaller, manageable portions called "buckets" to identify data partitioning as it's being written down. This feature is intended to improve the efficiency of reading same data. This efficiency improvement is mostly about getting rid of shuffles (also called exchanges) in join and aggregation queries.

> Apache Hive popularised buckets, and Apache Spark added their support.

Shuffle is a very expensive operation as it moves the data between executors or even across worker nodes in a cluster. Hence, it should be avoided wherever feasible. When there is a problem with the performance of Spark jobs, we should examine the transformations that involve shuffling.

With bucketing, we can pre-shuffle and store the data in a pre-shuffled form. It controls the physical arrangement of the data, thus we shuffle the data beforehand to prevent having to do it later on.

## Sorting the bucket

In addition to bucketing, we can also do sorting, which sorts each bucket according to the given fields. However, the opposite is not possible; we cannot sort without bucketing.

## Configure bucketing

Note that bucketing is enabled by default. Spark controls whether or not it should be enabled using the configuration property `spark.sql.sources.bucketing.enabled`. 

## How to create buckets?

In Spark API, the bucketBy method can be used for this purpose. 

```python
bucketBy(n, field1, field2, ...)
```

The first argument specifies the number of buckets to generate. The number of buckets is equal to the number of files generated. 

```python
# In Python
df.write\
    .bucketBy(16, "key")\
    .sortBy("value")\
    .saveAsTable("table_name")
```

## Bucketing applicable only to persistent tables

Only persistent tables can be used for bucketing and sorting. We can only use the `saveAsTable` method; we can't use the `save` method.
