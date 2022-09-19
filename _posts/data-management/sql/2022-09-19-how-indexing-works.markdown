---
layout: post
title:  "How Does Indexing Work?"
kicker: "Database"
subtitle: "PostgreSQL is an open source object-relational database system. It runs on all major operating systems, and it's ACID-compliant."
image: assets/images/posts-cover-images/postgres-basics.jpg
author: senthil
published_on: 2022-09-18 00:00:00 +0530
tags: ["database", "database-indexing", "sql"]
categories: database
featured: false
hidden: true
toc: false
---

# What is indexing in database?

Indexing in a database is a technique that makes use of data structures in order to reduce reduce the amount of time needed to search for item(s) within a table in a database. It helps in faster data retrieval from the database table. Additionally, it allows us to reduce the number of rows or records that need to be  analyzed.

Let's say we have a table called `Employee`, and two of its fields are named `Employee_Id` and `Employee_Name`. On this table, we wish to run the select query shown in the following example:

```sql
SELECT * FROM employee WHERE employee_name = "John";
```

When this query is processed, each and every row is examined to see whether or not the employee's name contains the name "John.

TODO: An index is an ordered representation of the indexed data.

> **Note:** It is important to keep in mind that indexing makes reading faster but writing slower! With every insert, update, and delete we perform, we also have to insert, update, and delete into the index. Therefore, as a general guideline, we should always strive to restrict the number of indices to a minimum.

## Ordered vs. unordered rows

If the table were ordered alphabetically, the process of locating the item that we are looking for would either be accelerated or slowed down, depending on which row it is present within the table. In addition, if we are successful in finding the item we were looking for, we might skip the remaining rows in the table.

In the event that the rows in the table are not organized in any particular fashion, the whole table will need to be scanned, a process that is more frequently referred to as a **full table scan**. You guessed correctly that doing a full table scan may take a considerable amount of time since it involves going over each row, and it is not very efficient.

Ordering is so important. Searching on ordered data is significantly more efficient than searching on unordered data.

# How indexing works?

Indexing is achieved by creating **index-table** or **index**. We will be able to create a proper index only when we have a clear understanding of our query as well as the criteria (table columns) that will be used to find the results.

This index will create a data structure based on a certain column (search column), and it will not store any other column in the data structure. Our data structure for the `Employee` table above will only contain the the `employee_name`.

The interesting question is what data is actually stored on the index. The values of the columns that we actually indexed are the only things that are stored in the index.  It's important to note that the index is not a copy of our table.

# Types of index

# Frequently asked questions (FAQ)

