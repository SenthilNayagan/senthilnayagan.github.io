---
layout: post
title:  "Need for Caching in Apache Spark"
kicker: "Apache Spark"
subtitle: "Caching is one of Spark's optimization strategies for reusing computations. It stores interim and partial results so they'll be utilised in subsequent computation stages."
image: assets/images/posts-cover-images/spark-caching-data-in-memory.jpg
author: senthil
published_on: 2022-07-24 22:00:00 +0530
tags: ["apache-spark", "cache", "data-caching"]
categories: apache-spark
featured: false
hidden: false
---

Caching is a common approach for reusing certain computation in Apache Spark. It's one of the optimisation techniques in Spark. Among big data practitioners, caching receives a lot of consideration and discussion.

The underlying data store is accessed each time an operation is performed in a Spark DataFrame, requiring the entire dataset to be sent across the network for each execution. When the same data is retrieved frequently, caching is immensely useful. The objective of caching is to *minimize disk I/O* and *retrieve data as quickly as possible* by storing it in RAM rather than on the disk.

Caching helps in storing interim and partial results so they'll be utilised in subsequent computation stages. These intermediate results are kept either in memory (by default) or on disk and are stored as RDDs. Obviously, data that has been cached in memory is faster to access, but cache space is always limited.

> **Run out of memory:** We will quickly exhaust our memory if we cache every RDD. We must thus carefully consider our options before choosing to cache an RDD.

# The art of caching
Caching frequently involves a lot of trial and error since it needs taking into account the number of available nodes, the importance of each RDD, and the amount of memory available for caching. As stated above, we will quickly exhaust our memory (resulting in an out-of-memory situation) if we cache every RDD. We must thus carefully consider our options before choosing to cache an RDD.

# Caching a DataFrame
In Spark, there are two functions that can be used to cache both RDD, DataFrame, and Dataset:
- `dataFrame.cache()`
- `dataFrame.persist(storageLevel)`

Cache and persist are distinct in that cache will store the RDD in memory while persist can cache at various storage levels.

## Persistence storage levels
There are various persistence storage levels, including:
- **MEMORY_ONLY** - Persist data on disk only in serialised format.
- **MEMORY_ONLY_SER**
- **MEMORY_ONLY_SER_2**
- **DISK_ONLY** - Persist data in memory only in deserialised format.
- **DISK_ONLY_2**
- **DISK_ONLY_3**
- **MEMORY_AND_DISK** - Persist data in memory, and if enough memory is not available, evicted blocks will be serialised to disk.
- **MEMORY_AND_DISK_2**
- **MEMORY_AND_DISK_SER**
- **MEMORY_AND_DISK_SER_2**
- **OFF_HEAP** - Data is persisted outside the heap called off-heap.

> **Need for off-heap storage type:** Bad memory management with data-intensive applications might cause the garbage collector to pause for long durations. Off-heap storage is not managed by the JVM's garbage collector. Therefore, it must be explicitly managed by the application.

# Caching a table
We should use `sqlContext.cacheTable("table_name")` in order to cache Spark SQL, or alternatively use `CACHE TABLE` table_name SQL query.

# Uncaching a DataFrame or table
The following are the ways to uncache:
- `dataFrame.unpersist()`
- `spark.catalog.uncacheTable("tableName")`
- `UNCACHE TABLE table_name`
- `spark.catalog.clearCache()` - It clears both DataFrames/tables