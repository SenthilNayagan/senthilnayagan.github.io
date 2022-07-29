---
layout: post
title:  "Introduction to Data Lake"
subtitle: ""
image: assets/images/posts-cover-images/introduction-to-data-lake.jpg
author: senthil
date:   2022-07-25 19:00:00 +0530
tags: ["data-engineering", "big-data", "data-lake", "lakehouse"]
categories: data-engineering data-lake
permalink: /:categories/:title
featured: false
hidden: true
---

# Data lake trade-offs
There are trade-offs involved in the shift from traditional data storage and processing platforms, such as databases and data warehouses, to data lakes. After the migration to the data lake, we have sacrificed the following capabilities in favor of others:
- We have given up durability and consistency features like ACID transactions in return for the ability to process them on a highly scalable platform.
- We have traded performance characteristics such as indexing and caching in exchange for the capacity to handle data in multiple formats.
- We have given up features like versioning and auditing in exchange for the ability to decouple storage and computing.