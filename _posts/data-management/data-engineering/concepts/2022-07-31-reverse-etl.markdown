---
layout: post
title:  "Reverse ETL"
kicker: "Data Engineering"
subtitle: "Reverse ETL refers to the process of feeding processed data from a data warehouse or data lake back into business applications, such as Salesforce, for use in business operations and forecasting."
image: assets/images/posts-cover-images/reverse-etl.jpg
author: senthil
published_on: 2022-07-31 00:00:00 +0530
tags: ["data-engineering", "reverse-etl", "etl"]
categories: data-engineering
featured: false
hidden: true
---

# Traditional approach
ETL, which stands for *extract, transformation, and load*, is a *traditional* data integration approach that takes data from several data sources, transforms it, and stores it in a centralized data repository. This centralized data repository might be a data warehouse or a data lake, and it would serve as the only reliable source of truth.

In this ETL-based method, data goes through staging and integration phases before arriving at either a data warehouse or a data lake as the final destination.

# What is reverse ETL?
We switched from ETL to ELT (Extract, Load, and Transform) as data volumes increased and we needed a faster way to load data into a data warehouse or data lake.