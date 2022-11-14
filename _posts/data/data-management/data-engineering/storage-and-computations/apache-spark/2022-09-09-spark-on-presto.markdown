---
layout: post
title:  "Presto on Apache Spark"
kicker: "Apache Spark"
subtitle: "lakeFS gives us the ability to manage our data lake in a way similar to that in which we manage our code."
image: assets/images/posts-cover-images/lakefs.jpg
author: senthil
date: 2022-10-07 00:00:00 +0530
tags: ["apache spark", "presto", "prestodb"]
categories: apache-spark
featured: false
hidden: true
toc: true
---

# Overview

> **Note:** The Presto referred to here is the PrestoDB, not the PrestoSQL or Trino.

# Presto's strengths and weaknesses

Presto has both strengths and weaknesses.

## Strengths

- It's ANSI SQL.
- Widely adopted.
- Interactive.
- Federated query design.
- Extensively used in scheduled (batch) workloads.

## Weaknesses

- Scale limitations.
- High memory query reliability.
- Long running query reliability.

# Frequently asked questions (FAQ)

## What is federated query engine?

A federated query is a way to send a query statement across data stored in various external data sources, such as relational, non-relational, object, or custom data sources. The federated query engine runs in a completely decoupled architecture, with computing on one side and storage on the other side.

What makes Query Federation such a game-changing breakthrough is its ability to simplify the process of accessing data from a variety of sources via the use of a single query. This is due to the fact that in the past, combining data from a variety of sources was a time-consuming and tedious procedure. In order to combine several data sources into a single, standardized format, we will need to use ETL operations.

Federated query engines are great for the infrequent analytics use cases where we can’t have the data in a single place and second-level performance isn’t important.

# Why optimized data warehouses are faster than federated query engine?

TODO