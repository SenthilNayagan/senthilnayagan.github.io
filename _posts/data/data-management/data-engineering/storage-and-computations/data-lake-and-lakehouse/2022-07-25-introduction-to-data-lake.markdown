---
layout: post
title:  "Introduction to Data Lake"
kicker: "Data Engineering"
subtitle: "A data lake is a centralized repository containing a significant amount of data from several sources in a more flexible natural or raw format for analytical usage."
image: assets/images/posts-cover-images/introduction-to-data-lake.jpg
author: senthil
published_on: 2022-07-25 19:00:00 +0530
tags: ["data-engineering", "big-data", "data-lake", "lakehouse"]
categories: data-lake-and-lakehouse
featured: false
hidden: true
---

> **Writing in progress:** If you have any suggestions for improving the content or notice any inaccuracies, please email me at [hello@senthilnayagan.com](mailto:hello@senthilnayagan.com). Thanks!

# Data lake trade-offs

There are trade-offs involved in the shift from traditional data storage and processing platforms, such as databases and data warehouses, to data lakes. After the migration to the data lake, we have sacrificed the following capabilities in favor of others:

- We have given up durability and consistency features like ACID transactions in return for the ability to process them on a highly scalable platform.
- We have traded performance characteristics such as indexing and caching in exchange for the capacity to handle data in multiple formats.
- We have given up features like versioning and auditing in exchange for the ability to decouple storage and computing.