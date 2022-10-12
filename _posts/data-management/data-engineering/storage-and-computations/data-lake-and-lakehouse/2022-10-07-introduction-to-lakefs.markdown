---
layout: post
title:  "Introduction to lakeFS"
kicker: "Data Lake"
subtitle: "lakeFS gives us the ability to manage our data lake in a way similar to that in which we manage our code."
image: assets/images/posts-cover-images/lakefs.jpg
author: senthil
date: 2022-10-07 00:00:00 +0530
tags: ["lakefs", "data-lake", "delta lake", "lakehouse"]
categories: data-lake
featured: false
hidden: true
toc: true
---

# What is lakeFS?

lakeFS is distributed as a single binary that contains several logical services. lakeFS offers a git-like version control interface for data lakes so that it gives us the ability to manage our data lake in a manner that is similar to how we manage our code.

lakeFS adapts the best practices from the field of software engineering to the field of data engineering.

## Collaboration

There is a need for an improved system that can version huge amounts of data. For team members to work collaboratively and concurrently on a data set that is rapidly evolving, they need a version (snapshot) of that data that is reserved exclusively for their use.

## Lineage

Obtaining data lineage has never been an easy operation, and it is now made much more complicated by the fact that each data set has numerous versions over time. If we did not provide the lineage details, it would be pointless.

## Data storage

lakeFS stores data in an underlying object store (GCS, ABS, S3, or any S3-compatible stores like MinIO or Ceph), with some of its metadata stored in PostgreSQL.

lakeFS provides the ability to branch, merge, rollback, etc.

# Frequently asked questions (FAQ)

## Difference between lakeFS and Delta Lake

