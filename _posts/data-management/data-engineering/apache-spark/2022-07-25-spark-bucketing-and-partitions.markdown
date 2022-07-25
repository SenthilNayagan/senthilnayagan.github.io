---
layout: post
title:  "Bucketing and Partitions in Spark"
subtitle: "Partitioning and bucketing are used to improve the reading of data by reducing the cost of shuffles, the need for serialization, and the amount of network traffic."
image: assets/images/posts-cover-images/spark-bucketing-and-partitions.jpg
author: senthil
date:   2022-07-25 15:00:00 +0530
tags: ["apache-spark", "partition", "bucketing"]
categories: data-engineering apache-spark partition-bucketing
permalink: /:categories/:title
featured: false
hidden: false
---

# Bucketing
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