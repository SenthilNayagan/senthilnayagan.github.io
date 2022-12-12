---
layout: post
title:  "Getting data into Apache Pinot"
kicker: "APACHE PINOT"
--subtitle: "Apache Pinot is a real-time, distributed OLAP datastore that was built for low-latency, high-throughput analytics, making it perfect for user-facing analytical workloads."
--image: assets/images/posts-cover-images/running-apache-pinot.jpg
author: senthil
date: 2022-12-07 00:00:00 +0530
tags: ["apache-pinot", "pinot", "olap", "olap-datastore"]
categories: apache-pinot
featured: false
hidden: true
toc: true
---

# Apache Pinot's file system

Let's first briefly discuss Pinot's file system. The file system is an abstraction layer that allows users to store and read data in a data layer of their choice for real-time and offline segments. Pinot uses distributed file systems (DFS). Pinot supports the following distributed file systems:

- **Amazon S3**
- **Google Cloud Storage**
- **Azure Data Lake Storage**
- **HDFS**

By default, Pinot doesn't have a storage layer, so if the system crashes, none of the data that was sent will be saved. To store the generated segments permanently in a local or distributed file system, we will have to change how the controller and server are set up to add **deep store**.

> **Deep store:** The deep store is the permanent store for segment files. It is used to make backups and get them back (restore). A cluster's new server nodes will get a copy of the segment files from the deep store. If the local segment files on a server get damaged in some way, a new copy will be pulled down from the deep store when the server is restarted. Note that the deep store stores a compressed version of the segment files, and it usually doesn't have any indexes. These compressed files can be kept on a local or a distributed file system. 

# Importing or ingesting data into Apache Pinot

We can either ingest **offline data** or **realtime data** into Pinot. To get both offline and real-time data into Pinot, we need a Pinot schema and a Pinot table configuration.

## Pinot schema

Schema is used to define the names, data types, and other details for the columns in a Pinot table.

The Pinot schema is composed of:

- **schemaName** - Defines the name of the schema. Most of the time, this is the same as the table name.
- **dimensionFieldSpec** - A dimensionFieldSpec is defined for each dimension column. Dimension columns are typically used in slice-and-dice operations such as `GROUP BY` and `WHERE`.
- **metricFieldSpec** - A metricFieldSpec is defined for each metric column. Aggregation is done with the help of metric columns. In the dialect of a data warehouse, these can also be called fact or measure columns. Some operation for which metric columns are used:
    - Aggregation - `SUM`, `MIN`, `MAX`, `COUNT`, `AVG`, etc.
    - Filter clause such as `WHERE`
- **dateTimeFieldSpec** - A dateTimeFieldSpec is defined for the time columns. Note that there can be multiple time columns in a table, but only one of them can be treated as primary. The primary time column is used by Pinot to maintain the time boundary between offline and real-time data in a hybrid table and for retention management. A primary time column is mandatory if the table's push type is `APPEND` and optional if the push type is `REFRESH`. Common operations that can be done on time column: `GROUP BY` and `WHERE`.

Pinot doesn't have strict rules about which of these categories these columns belong to. Instead, you can think of the categories as indications that Pinot can use to do internal optimizations. When we do segment merges and rollups, the categories are also important. Pinot uses the time and dimension fields to figure out which records to merge or roll up.

### Data types

Data types determine the operations that can be performed on a column. Pinot supports the following data types:

- `INT`
- `LONG`
- `FLOAT`
- `DOUBLE`
- `BIG_DECIMAL`
- `BOOLEAN`
- `TIMESTAMP`
- `STRING`
- `JSON`
- `BYTES`

> **No explicit type exists for lists or arrays:** Pinot also works with columns that have lists or arrays of items, but there isn't a specific data type for these lists or arrays. We can instead say that a dimension column can take more than one value.

### Sample schema

Here's an example of a Pinot schema for an `employee` table:

```json
{
  "schemaName": "employee",
  "dimensionFieldSpecs": [
    {
      "name": "id",
      "dataType": "LONG"
    },
    {
      "name": "name",
      "dataType": "STRING",
    },
    {
      "name": "age",
      "dataType": "INT",
    }
  ],
  "metricFieldSpecs": [
    {
      "name": "salary",
      "dataType": "DOUBLE",
      "defaultNullValue": 0
    }
  ],
  "dateTimeFieldSpecs": [
    {
      "name": "createdOn",
      "dataType": "LONG",
      "format": "EPOCH",
      "granularity": "15:MINUTES"
    },
    {
      "name": "modifiedOn",
      "dataType": "LONG",
      "format": "EPOCH",
      "granularity": "15:MINUTES"
    }
  ]
}
```

## Pinot table configuration

Table configuration is used to define the table properties, such as **name**, **type**, **indexing**, **routing**, **retention**, etc. It is written in JSON format and is stored in ZooKeeper, along with the table schema.

### Sample table configuration

A sample table configuration for an employee table is shown below. The `tableType` property shows us that it is an offline table.

```json
{
  "tableName": "employee",
  "segmentsConfig" : {
    "timeColumnName": "createdOn",
    "timeType": "MILLISECONDS",
    "replication" : "2",
    "schemaName" : "employee"
  },
  "tableIndexConfig" : {
    "invertedIndexColumns" : [],
    "loadMode"  : "MMAP"
  },
  "tenants" : {
    "broker": "DefaultTenant",
    "server": "DefaultTenant"
  },
  "tableType": "OFFLINE",
  "metadata": {}
}
```

## Ingesting offline data

Segments for offline tables are constructed *outside of Pinot*, usually in Hadoop via map-reduce jobs and ingested into Pinot via REST API provided by the Controller. Pinot has libraries that can create Pinot segments from input files in AVRO, JSON, or CSV formats in a Hadoop job and send them to the controllers via REST APIs.

## Ingesting realtime data

TODO

