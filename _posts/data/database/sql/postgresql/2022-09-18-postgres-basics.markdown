---
layout: post
title:  "PostgreSQL Basics"
kicker: "PostgreSQL"
subtitle: "PostgreSQL is an open source object-relational database system. It runs on all major operating systems, and it's ACID-compliant."
image: assets/images/posts-cover-images/postgres-basics.jpg
author: senthil
published_on: 2022-09-18 00:00:00 +0530
tags: ["postgresql", "postgres", "sql"]
categories: postgresql
featured: false
hidden: true
toc: true
---

# Overview

PostgreSQL is an open source object-relational database system. It runs on all major operating systems, and it's ACID-compliant. PostgreSQL is highly extensible, which means that we can define our own data types, create custom functions, and even write code in different programming languages without having to recompile our database!

Even if many of the features required by the SQL standard are supported, some of them may *have slightly different syntax or function*. As of the version 14 release in September 2021, PostgreSQL meets at least 170 of the 179 required features for **SQL:2016 Core compliance**. Note that no relational database meets full conformance with this standard. JSON features set it apart from some of the most widely used NoSQL databases, despite the fact that it is a relational database, like Oracle, MySQL, or SQL Server databases.

To ensure durability, PostgreSQL writes all its transactions to a **Write-Ahead Logging** (**WAL**) segment on disk, to ensure that a committed transaction is safe if a crash occurs.

> **Write-Ahead Logging (WAL)** is a method that is commonly used for *transaction logging*. The idea is that any modifications made to data files should only be written to disk after they have been logged to permanent storage. When we follow this approach, we eliminate the requirement to flush data pages to disk on every transaction commit. This is because we are certain that in the event of a crash, we will be able to restore the database by using the logs. Refer [below]({{ site.baseurl }}/postgresql/2022/postgres-basics#what-is-write-ahead-logging) for more information.

# Clusters in PostgreSQL

Before we can do anything, we need to setup a storage space on the disk for our database. This is what we refer to as a **database cluster**. A database cluster is a collection of databases that is managed by a single instance of a running database server.

A database cluster, after it has been initialized, will have a database by the name of **postgres**. This database serves as the default database and is intended to be used by utilities, users, and third party applications. Note that the existence of the postgres database is not strictly necessary for the database server itself; however, many other external utility applications do presume that it is there.

A database cluster, when speaking in terms of file systems, is just a single directory under which all of the data will be kept. We call this the *data directory* or *data area*. It's completely up to us where (location) we choose to store our data. Before the data directory can be used, it has to be initialized with the help of the program `initdb`, which is included with PostgreSQL by default.

To initialize a database cluster manually, run `initdb` and specify the desired file system location of the database cluster with the `-D` option, for example:

```bash
$ initdb -D /usr/local/pgsql/data
```


# Frequently asked questions (FAQ)

## What is Write-Ahead Logging (WAL)?

Write-Ahead Logging (WAL) is a method that is commonly used for *transaction logging*. The idea is that any modifications made to data files should only be written to disk after they have been logged to *permanent storage*. When we follow this approach, we eliminate the requirement to flush data pages to disk on every transaction commit. This is because we are certain that in the event of a crash, we will be able to restore the database by using the logs.

With the help of WAL, we can be able to do both roll-forward and roll-backward recoveries:

- **Roll-forward recovery**, also known as REDO, is the process by which any changes that have not been applied to the data pages will first be redone from the log records. 
- **Roll-backward recovery**, also known as UNDO, is the process by which any changes made by uncommitted transactions will be deleted from the data pages.

In addition, WAL is used for replication.

### Benefits of WAL

- WAL significantly reduces the number of disk writes since only the log file needs to be flushed to disk at the time of transaction commit.
- Data consistency. With WAL, database is able to guarantee consistency in the case of a crash.

## How to identify the location of the data directory in the simplest way possible?

Use `ps` command to identify PostgreSQL cluster:

```bash
$ ps -eaf | grep /post
```

**Output:**

```bash
501  8874     1   0  3:40PM ??         0:00.20 /Applications/Postgres.app/Contents/Versions/14/bin/postgres -D /Users/john/Library/Application Support/Postgres/var-14 -p 5432
```

The output reveals that the data directory may be found at the location indicated by the `-D` notation.