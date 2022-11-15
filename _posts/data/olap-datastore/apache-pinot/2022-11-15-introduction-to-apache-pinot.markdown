---
layout: post
title:  "Introduction to Apache Pinot"
kicker: "APACHE PINOT"
subtitle: "Apache Pinot is a real-time, distributed OLAP datastore that was built for low-latency, high-throughput analytics, making it perfect for user-facing analytical workloads."
image: assets/images/posts-cover-images/elasticsearch.jpg
author: senthil
date: 2022-11-15 00:00:00 +0530
tags: ["apache-pinot", "pinot", "olap", "olap-datastore"]
categories: apache-pinot
featured: false
hidden: true
toc: true
---

# Overview

Apache Pinot is a *real-time*, *distributed OLAP datastore* that was built for low-latency, high-throughput analytics, making it perfect for user-facing analytical workloads. It can ingest directly from streaming data sources like Apache Kafka and Amazon Kinesis and make the events available for querying right away. It can also ingest from batch data sources such as Hadoop HDFS, Amazon S3, Azure ADLS, and Google Cloud Storage.

At the heart of the system is a *columnar store* with smart indexing and pre-aggregation techniques for low latency. This makes Pinot the best choice for real-time analytics.

|![Apache Pinot Overview](/assets/images/posts/pinot-overview.png)|
|:-:|
|<sup>*Figure 1: Apache Pinot Overview. Image Courtesy: https://docs.pinot.apache.org*</sup>|<br/><br/>|

# Getting data into Pinot

To get the data into Pinot, we need a **Pinot schema** and a **Pinot table**.

## Pinot schema

The schema catagorizes columns into:

- **Dimensions** (dimensionFieldSpecs)
- **Metrics** (metricFieldSpecs)
- **Time** (timeFieldSpecs)

```text
{
  "schemaName": "sample",
  "dimensionFieldSpecs": [..]
  "metricFieldSpecs": [..]
  "timeFieldSpecs": {..}
}
```

### Schema dimensions

A sample schema with the dimensions (`dimensionFieldSpecs`) `studentID`, `firstName`, `lastName`, `gender`, and `subject` is shown below:

```json
{
  "schemaName": "sample",
  "dimensionFieldSpecs": [
    {
      "name": "studentID",
      "dataType": "INT"
    },
    {
      "name": "firstName",
      "dataType": "STRING"
    },
    {
      "name": "lastName",
      "dataType": "STRING"
    },
    {
      "name": "gender",
      "dataType": "STRING"
    },
    {
      "name": "subject",
      "dataType": "STRING"
    }
  ],
  "metricFieldSpecs": [
    {
      "name": "score",
      "dataType": "STRING"
    }
  ],
  "timeFieldSpecs": {
    "incomingGranularitySpec": {
      "name": "timestamp",
      "dataType": "LONG",
      "timeFormat": "EPOCH",
      "timeType": "MILLISECONDS"
    }
  }
}
```

## Pinot Table

A table refers to a collection of data consisting of rows and columns. This concept is similar to that of other databases.

Let's look at a sample table config file shown below. The table config file has a `tableName`, a `tableType`, which in this case is "OFFLINE," and some other information such as `schemaName`, `timeColumnName` and `replication`.

The `tenants` section is where we define which tenant this table will belong to. It implies that this table's data and queries only use the brokers and servers tagged `DefaultTenant`. This allows us to achieve isolation between tables if necessary and eliminates the need to create multiple clusters for every use case.

```json
{
  "tableName": "sample",
  "segmentsConfig": {
    "timeColumnName": "timestamp",
    "timeType": "MILLISECONDS",
    "replication": "2",
    "schemaName": "sample"
  },
  "tableIndexConfig": {
    "invertedIndexColumns": [
      
    ],
    "loadMode": "MMAP"
  },
  "tenants": {
    "broker": "DefaultTenant",
    "server": "DefaultTenant"
  },
  "tableType": "OFFLINE",
  "metadata": {
    
  }
}
 ```

# Pinot Controller

Pinot uses [Apache Helix](https://helix.apache.org/){:target="_blank"} for cluster management. Pinot controller hosts Apache Helix, and together they are responsible for managing all the other components of the cluster.

# Pinot Broker

Brokers handle Pinot queries. They accept queries from clients and forward them to the right servers. They gather results from the servers and combine them into a single response to send back to the client.

|![Broker interaction with other components](/assets/images/posts/apache-pinot-broker-interactions.jpg)|
|:-:|
|<sup>*Figure 2: Broker interaction with other components. Image Courtesy: https://docs.pinot.apache.org.*</sup>|<br/><br/>|

# Get started with Pinot

## Starting ZooKeeper

We will start the ZooKeeper using the pinot-admin script (`pinot-admin.sh`), which can be found in the Apache Pinot installed directory.

```shell
pinot-admin.sh StartZookeeper -zkPort 2181
```

## Starting Pinot controller

Pinot controller hosts Apache Helix, and together they are responsible for managing all the other components of the cluster.

```shell
pinot-admin.sh StartController -zkAddress localhost:2181 -clusterName PinotCluster -controllerPort 9001
```

In the above command, we are starting the Pinot controller on port 9001. We can give any name to a cluster using the `-clusterName` option.

Let's look at the ZooInspector tool to see what changes show up after starting the Pinot controller. We have a new cluster called PinotCluster which has cluster-level config properties.

|![ZooInspector Tool: New Cluster, PinotCluster is showing up](/assets/images/posts/zooinspector-controller.png)|
|:-:|
|<sup>*Figure 2: ZooInspector Tool: New Cluster, PinotCluster is showing up.*</sup>|<br/><br/>|

We have a participants directory that lists all of the cluster participants. So far, we only have the controller that we just started.

Let's add another controller, and this time we'll use a different controller port, `9002`:

```shell
pinot-admin.sh StartController -zkAddress localhost:2181 -clusterName PinotCluster -controllerPort 9002
```

Let's get back to the ZooInspector tool. Our second controller can be found in the participant directory. In the controller directory, we can see a leader node, which tells us which of the two controllers is the lead controller. The lead controller has additional responsibilities, such as running some periodic maintenance and cleanup tasks in the background.

|![ZooInspector Tool: Shows another controller under participant directory](/assets/images/posts/zooinspector-2nd-controller.png)|
|:-:|
|<sup>*Figure 3: ZooInspector Tool: Shows another controller under participant directory.*</sup>|<br/><br/>|

Let's see what else our controller can do. Type `localhost:9001` into the web browser's address bar. This opens the dashboard for the Pinot cluster, which is shown below:

|![Apache Pinot - Cluster Dashboard](/assets/images/posts/apache-pinot-cluster-dashboard.png)|
|:-:|
|<sup>*Figure 3: Apache Pinot - Cluster Dashboard.*</sup>|<br/><br/>|

This dashboard has the following options:

- **Cluster Manager**
- **Query Console** - lets us run queries on the tables in our cluster.
- **ZooKeeper Browser**
- **Swagger REST API** - has admin endpoints to operate and manage the cluster. Here we can perform read/write/delete operations on other entities of a cluster.

Below is the Swagger REST API page:

|![Swagger REST API Page](/assets/images/posts/swagger-rest-api-screen.png)|
|:-:|
|<sup>*Figure 5: Swagger REST API Page.*</sup>|<br/><br/>|

## Starting Pinot broker

Brokers handle Pinot queries. They accept queries from clients and forward them to the right servers. They gather results from the servers and combine them into a single response to send back to the client.

```shell
```

# Frequently asked questions (FAQ)
 
## What is tenant in Apache Pinot?

Tenant is one of the core components of Apache Pinot. It's simply a logical grouping of nodes formed by giving the nodes the same Helix tag. In our cluster, we have a default tenant called "default tenant." When nodes are created in the cluster, they automatically get added to the default tenant.

## What is the Ideal State and Exteral View?

Ideal State represents the *desired state* of the table resource. The Exteral View represents the *actual state*. Any cluster action usually updates the Ideal State, and then Helix sends state transitions to the right components.

## How does Pinot use Zookeeper?

The cluster management in Pinot is made with **Apache Helix**, which is built on top of Zookeeper. Helix uses Zookeeper to store the cluster state, including Ideal State, External View, Participants, etc. Besides that, Pinot uses Zookeeper to store other information such as Table configs, schema, Segment Metadata, etc.

## What is Apache Helix?

Apache Helix is a generic open-source cluster management framework developed by LinkedIn. It is used for the automatic management of *partitioned*, *replicated* and *distributed resources* hosted on a cluster of nodes.

Helix automates:

- Reassignment of resources when a node fails
- Cluster expansion
- Reconfiguration

## What is Helix task?

In Helix terminology, a task is referred to as a *resource*. Helix is based on the idea that every task has the following attributes associated with it:

- **Location** (e.g. it is available on Node N1)
- **State** (e.g. it is running, stopped etc.)

 