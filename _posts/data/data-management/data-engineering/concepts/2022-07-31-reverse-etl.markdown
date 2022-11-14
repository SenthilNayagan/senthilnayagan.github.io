---
layout: post
title:  "Reverse ETL"
kicker: "Data Engineering"
subtitle: "Reverse ETL is the process of feeding processed data (gold-layer tables) from a data warehouse or data lake back into business applications, primarily for use in operational analytics domains such as sales, marketing, etc., in order to enable business operations and forecasts."
image: assets/images/posts-cover-images/reverse-etl.jpg
author: senthil
date: 2022-07-31 00:00:00 +0530
tags: ["data-engineering", "reverse-etl", "etl"]
categories: data-engineering
featured: false
hidden: true
---

Before we dive deep into the subject, let's clarify *operational analytics* briefly. Operational analytics is a subset of data analytics that aims to improve business operations. Business operations are the everyday activities businesses participate in to improve their value and generate a profit. 

The primary distinction between operational analytics and other forms of analytics is that *operational analytics is analytics on the fly*, which means that data emerging from different segments of an organization is analyzed in real-time to feed back into the organization's immediate decision-making. Some people refer to operational analytics as *continuous analytics*, which is another way of highlighting the continuous digital feedback loop that can exist between different components of an organization.

# Traditional approach
ETL, which stands for *extract, transformation, and load*, is a *traditional* data integration approach that takes data from several data sources, transforms it, and stores it in a centralized data repository. This centralized data repository might be a data warehouse or a data lake, and it would serve as the only reliable source of truth.

In this ETL-based method, data goes through staging and integration phases before arriving at either a data warehouse or a data lake as the final destination.

# What is reverse ETL?
We switched from ETL to ELT (Extract, Load, and Transform) as data volumes increased and we needed a faster way to load data into a data warehouse or data lake.