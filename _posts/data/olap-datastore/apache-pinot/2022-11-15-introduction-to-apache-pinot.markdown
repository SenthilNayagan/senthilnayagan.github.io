---
layout: post
title:  "Apache Pinot joins hands with Kafka and Presto to provide low-latency, high-throughput user-facing analytics"
kicker: "APACHE PINOT"
subtitle: "Apache Pinot is a real-time, distributed OLAP datastore that was built for low-latency, high-throughput analytics, making it perfect for user-facing analytical workloads. Pinot joins hands with Kafka and Presto to provide user-facing analytics."
image: assets/images/posts-cover-images/presto-pinot-kafka.png
author: senthil
date: 2022-11-15 00:00:00 +0530
tags: ["apache-pinot", "pinot", "olap", "olap-datastore"]
categories: apache-pinot
featured: false
hidden: false
toc: true
draft: true
---

This post is for you if you are curious about Apache Pinot but are just getting started with it. We'll go a little deeper into Apache Pinot to help you understand the components that make up a Pinot cluster. We will also get hands-on experience by running samples that include both streaming and batch data imports into Pinot and then analytical query executions.

# The evolution of analytics

## Batch analytics

In the past, analytics were often performed in batches, resulting in high-latency analytics where queries returned responses based on data that was at least minutes, hours, or even days old, depending on the volume of data and available computing resources. The generation of **business intelligence** (**BI**) reports is one use case of batch analytics. Business intelligence uses historical data to report on business trends and answer strategic questions. In batch-style analytics, jobs are generally scheduled to run at night or during non-business hours. So, it often provides us with insights *after the fact*. Most of the time, these batch-type insights are based on stale data (old information), so we can't rely on them. So no one anymore wants to do analytics in batches.

## Real-time analytics

Real-time analytics has now become something that every business ought to do. It's the process of applying logic to data to get insights or draw conclusions *right away* so that better decisions can be made. "Real-time" in real-time analytics means being able to get business insights *as soon as possible* after data (transactions) enters the system. Having access to analytics in real-time is important for day-to-day operations, financial intelligence, triaging incidents, and allowing businesses to act quickly. The most crucial benefit of real-time analytics is that it allows us to take opportunities and stop problems before they happen.

## User-facing analytics

In the world we live in now, everyone needs real-time analytical data, not just business analysts or top-level executives. We call this kind of analytics "**user-facing analytics**" also known as "**customer-facing analytics**." One good example of this is LinkedIn's "Who viewed your profile" feature, which lets all of its more than 700 million users slice and dices the information about who looked at their pages. Uber created the UberEats Restaurant Manager app to give restaurant owners real-time insights into their order data. This is yet another excellent example of how the best use of user-facing real-time analytics improved the end-user experience. 

In user-facing analytics, users won't put up with painfully slow analytics. When they can find insights in real-time, they are more open to a data-driven culture. So, we need a solution that can scale to millions of users and offer fast, real-time insights. Businesses are working hard to speed up the steps needed to get enough data to answer everyone's questions. One such solution that comes to our rescue is "**Apache Pinot**."

|![The Evolution of Analytics](/assets/images/posts/batch-analytics-to-user-facing-analytics.png)|
|:-:|
|<sup>*Figure 1: The Evolution of Analytics.*</sup>|<br/><br/>|

---

# A brief introduction to Apache Pinot

Apache Pinot is a **real-time**, **distributed OLAP datastore** that was built for low-latency, high-throughput analytics, making it perfect for user-facing analytical workloads.

It can ingest directly from streaming data sources like Apache Kafka and Amazon Kinesis and make the events available for querying right away. It can also ingest from batch data sources such as Hadoop HDFS, Amazon S3, Azure ADLS, and Google Cloud Storage.

At the heart of the system is a *columnar store* equipped with advanced indexing and pre-aggregation techniques for low latency. This makes Pinot the best choice for real-time analytics.

One of the best things about Pinot is that it has a pluggable architecture. The plugins make it easy to add support for any third-party system, such as an *execution framework*, a *filesystem*, or an *input format*. For example, some plugins make it easy to ingest data and push it to our Pinot cluster:

- `pinot-batch-ingestion-spark`
- `pinot-s3`
- `pinot-parquet`

|![Apache Pinot Overview](/assets/images/posts/pinot-overview.png)|
|:-:|
|<sup>*Figure 2: Apache Pinot Overview. Image Courtesy: https://docs.pinot.apache.org.*</sup>|<br/><br/>|

---

# Pinot joins hands with Presto and Kafka

Now, the question comes: **Can't Pinot just do what it's supposed to do without help from Kafka and Presto?** Due to some limitations, Pinot depends on Kafka and Presto to provide user-facing analytics with high throughput and low latency. We’ll go through the reasons for using Kafka and Presto with Pinot, and how they complement each other.

## Need for Presto

Presto and Pinot are distinct technologies, yet they complement each other quite well for conducting and storing ad-hoc data analytics. Presto supports SQL, but users can't use it to get fresh aggregated data. Pinot, on the other hand, can give us *second-level data freshness*, but it doesn't support flexible queries.

> **Second-level data freshness:** Second-level data freshness is the amount of time between when the organisation gets the data and when it can be used for in-depth analytics.

|![Pinot vs. Presto](/assets/images/posts/pinot-vs-presto.png)|
|:-:|
|<sup>*Figure 3: Pinot vs. Presto.*</sup>|<br/><br/>|

## Need for Kafka

Apache Kafka has become the industry standard for real-time event streaming because it flawlessly addresses the issue of real-time ingestion of data with high velocity, volume, and variability. Integrating Kafka with Pinot makes the event streams available for querying in real-time. This allows segments to be available for query processing as they’re being built.

In the following sections, we will go over how Pinot leverages Kafka and Presto, making it perfect for user-facing analytical workloads at scale.

---

# Taking a closer look into Pinot and its components

|![Pinot Architecture](/assets/images/posts/apache-pinot-architecture.png)|
|:-:|
|<sup>*Figure 4: Pinot Architecture.*</sup>|<br/><br/>|

Pinot has two kinds of components: 

- Logical components
- Architectural components

## Logical components

Pinot cluster has the following logical components:

- **Tenant**
- **Table**
- **Segment**

A logical view is another way to see what the cluster looks like:

|![Pinot Cluster's Logical View](/assets/images/posts/apache-pinot-cluster-logical-view.png)|
|:-:|
|<sup>*Figure 5: Pinot Cluster's Logical View.*</sup>|<br/><br/>|

- A cluster contains tenants
- Tenants contain tables
- Tables contain segments

### Tenant

A tenant is a *logical* component of Apache Pinot. It’s simply a logical grouping of resources (servers and brokers) with the same Helix tag. So, tenant enable us to group resources that are used for isolation. In our cluster, we have a default tenant called “default tenant.” When nodes are created in the cluster, they automatically get added to the default tenant.

Pinot has top-notch support for tenants so that *multi-tenancy* can work. Every table is associated with a server tenant and a broker tenant. This sets the nodes that will be used as servers and brokers by this table. This lets all of the tables for a certain use case be put together under a single tenant name.

The idea of tenants is very important when Pinot is used for many different purposes and there is a need to set limits or separate tenants (isolation) in some way.

### Table

A table is a logical abstraction that represents a *collection of related data*. It is made up of rows and columns (known as documents in Pinot). This concept is similar to that of other databases. A schema defines the table's columns, data types, and other metadata.

Pinot supports the following types of table:

- **Offline** - Offline tables ingest pre-built pinot-segments from external data stores. This is generally used for *batch ingestion*.
- **Real-time** - Real-time tables ingest data from streams such as Kafka and build segments from the consumed data.
- **Hybrid** - Under the hood, a hybrid Pinot table is made up of both real-time and offline tables. All tables in Pinot are of the hybrid type by default.

> The user who is querying the database doesn't need to know what kind of table it is. In the query, they only need to say the name of the table. Regardless of whether we have an offline table `myTable_OFFLINE`, a real-time table `myTable_REALTIME`, or a hybrid table containing both of these, the query will be:<br/><br/>`select count(*) from myTable`.<br/>

**Table configuration** is used to define the table properties, such as **name**, **type**, **indexing**, **routing**, **retention**, etc. It is written in JSON format and is stored in ZooKeeper, along with the table schema.

### Segment

Pinot breaks a table into multiple small *chunks of data* known as **segments** that are distributed across Pinot servers. These segments can also be stored in a **deep store** such as HDFS. Pinot segment is a unit of *partitioning*, *replication*, and *query processing* that represents a subset of the input data along with the specified indices. By using Apache Helix, the Pinot controller defines how data is partitioned and replicated across servers. Using this information, the Pinot broker disperses queries to individual servers and gather them back together.

There are two types of segments:

1. Mutable segments
2. Immutable segments

#### Mutable segments

Mutable segments are those that are being stored in memory in the `CONSUMING` state. Each mutable segment organizes the incoming data into a columnar format and updates the needed indices, such as inverted or text indexes, in real time. The mutable segments are available for query processing right away, as they’re being built. Therefore, given the low ingestion overhead, Pinot provides the same level of data freshness as Kafka.

#### Immutable segments

Each server decides on its own when it is time to save the in-memory segments to disk, based on a set of criteria. Such on-disk segments are referred to as immutable segments.

The criteria used for creating immutable segments can either be:

- the amount of time elapsed since the segment was initially created
- the number of rows consumed

> **Note:** For offline tables, segments are built outside of pinot and uploaded using a distributed executor such as Apache Spark or Hadoop. For real-time tables, segments are built in a specific interval inside Pinot.

A table is represented as a [**Helix resource**](https://helix.apache.org/Concepts.html){:target="_blank"} in the Pinot cluster, and each segment of a table is represented as a [**Helix Partition**](https://helix.apache.org/Concepts.html){:target="_blank"}.

|![Tenant -> Tables -> Segments](/assets/images/posts/apache-pinot-tenant-table-segment.png)|
|:-:|
|<sup>*Figure 6: Tenant -> Tables -> Segments.*</sup>|<br/><br/>|

## Architectural components

A Pinot cluster is a set of nodes comprising of:

- **Server**
- **Broker**
- **Controller**
- **Minion**

Pinot uses [**Apache Helix**](https://helix.apache.org/){:target="_blank"} for cluster management. Helix handles resources that are replicated and partitioned in a distributed system. Helix uses **ZooKeeper** to store cluster state and metadata.

### Server

Servers host (store) the data segments and serve queries based on the data they host. Pinot servers consume data directly from the Kafka topic. They are also indexing the data as they ingest it, and they serve queries on the data that they have.

There are two types of servers:

- **Offline server**
- **Real-time server**

#### Offline server

Offline servers download segments from the segment store so that they can host and serve queries. When a new segment is uploaded to the controller, the controller decides which servers will host the new segment and notifies them to download the segment from the segment store. When the servers get this notification, they download the segment file and put the segment on the server so that they can handle queries.

|![Offline Server shows Batch Ingestion](/assets/images/posts/apache-pinot-batch-ingestion.png)|
|:-:|
|<sup>*Figure 7: Offline Server shows Batch Ingestion.*</sup>|<br/><br/>|

#### Real-time server

Real-time servers ingest directly from a real-time stream like Kafka. Based on certain thresholds, they make segments of the data that has been stored in-memory from time to time. Then, this segment is saved to the segment store.

|![Real-time Ingestion](/assets/images/posts/apache-pinot-realtime-ingestion.png)|
|:-:|
|<sup>*Figure 8: Real-time Ingestion.*</sup>|<br/><br/>|

### Broker

Broker handles Pinot queries. They accept queries from clients and forward them to the right Pinot servers. They gather results from the servers and combine them into a single response to send back to the client.

|![Broker interaction with other components](/assets/images/posts/apache-pinot-broker-interactions.png)|
|:-:|
|<sup>*Figure 9: Broker interaction with other components.*</sup>|<br/><br/>|

### Controller

The node that observes and controls the participant nodes. In other words, the Pinot controller manages all the components of the Pinot cluster with the help of Apache Helix. It is in charge of coordinating all cluster transitions and making sure that state constraints are met while keeping the cluster stable. **Pinot controllers** are modeled as controllers.

The Pinot controller is really the brains of the cluster, and it takes care of cluster membership, figuring out what data is located on which server, and performing query routing. Pinot controller hosts Apache Helix (for cluster management), and together they are responsible for managing all the other components of the cluster.

The Pinot controller has a user interface that lets us access and query Pinot tables. We can also use this user interface to add, edit, or delete schema and table configurations. We can access the user interface via controller's port (`http://localhost:9000`); port `9000` is the default controller's port.

### Minion

I'm not going into Minion details here. If you'd like to learn more about Minion, check out the official document [here](https://docs.pinot.apache.org/basics/components/minion){:target="_blank"}.

<!--Helix divides nodes into logical components based on their responsibilities:

#### Participant

The nodes that host resources (tasks) that are distributed and partitioned. **Pinot servers** are modeled as participants.

#### Spectator

The nodes that keep track of *current state* of each participant and use this information to access the resources. Spectators are notified when the state of the cluster changes (state of a participant, or that of a partition in a participant). **Pinot brokers** are modeled as spectators.



## Table

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
 -->

 ---

# Pinot's use cases and limitations

## Use cases

Pinot was built to execute real-time OLAP queries on massive amounts of streaming data and events with low latency. Pinot also supports batch use cases with a similar guarantee of low latency. It works well in situations where we need to do quick analytics, such as aggregations, on immutable data that's being received via real-time data ingestion. Also, Pinot is a great choice for querying time series data with lots of dimensions and metrics.

## Limitations

- Pinot is not a replacement for a database and should not be used as a source of truth. Pinot is not a replacement for a search engine.

### SQL query limitations:
  - Pinot does not fully support ANI-SQL at this time. 
  - It doesn't allow cross-table queries (using JOINs) as well as nested queries. 
  - It also cannot handle a large amount of data shuffle. 
  - Table joins and other operations, such as a large amount of data shuffling, may be accomplished using either the Trino-Pinot connector or the Presto-Pinot connector.

---

# Getting started with Pinot

This section describes how to launch a Pinot cluster and ingest data into it.

## Running Pinot components

Apache Pinot can be run in any of the following environments:

- **locally** on our own computer 
- in **Docker**
- in **Kubernetes**

[Read more]({{ site.baseurl }}/apache-pinot/2022/running-apache-pinot-locally){:target="_blank"} to find out how to deploy and run Apache Pinot locally on our computer.

## Getting data into Pinot

There are multiple ways of importing data into Pinot. [Read more]({{ site.baseurl }}/apache-pinot/2022/getting-data-into-pinot){:target="_blank"}.

---

# Frequently asked questions (FAQ)

## What are the challenges that user-facing analytics have to deal with?

- **Latency:** Must be in the sub-second range.
- **Throughput:** Should support millions of concurrent users querying the data.
- **Data freshness:** Insights must be fresh (current), up to date, and relevant.

## What is queries per second (QPS)?

Queries per second (QPS) is a way to measure how many searches a system for obtaining information, like a search engine or a database, gets in one second. The term is often used for any request-response system, in which case it should be called **requests per second** (**RPS**).

## What is the Ideal State and Exteral View?

Ideal State represents the *desired state* of the table resource. The Exteral View represents the *actual state*. Any cluster action usually updates the Ideal State, and then Helix sends state transitions to the right components.

## How does Pinot use ZooKeeper?

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

## Is Pinot support open distributed data file format like Parquet?

No. A closed-file format is used by Pinot. In the future, they might be able to work with open file formats like Parquet.

## What input file formats can be used to send data to Pinot?

- **CSV**
- **Parquet**
- **ORC**
- **AVRO**
- **JSON**
- **Thrift**
- **Protocol Buffers**

## How can data from Pinot be retrieved?

There are different ways to query for data from Pinot using:

- Broker endpoint (REST API)
- Query console
- Pinot-admin
- Pinot clients

### Broker endpoint

We can use the `/query/sql` endpoint on a broker to access the Pinot REST API using the POST operation with a JSON body that includes the parameter sql.

```bash
$ curl -H "Content-Type: application/json" -X POST \
   -d '{"sql":"select foo, count(*) from myTable group by foo limit 100"}' \
   http://localhost:8099/query/sql
```

### Query console

Query Console can be used for running ad-hoc queries. The Query Console can be accessed by entering the `<controller host>:<controller port>` in our browser.

### Pinot-admin

We can also query using the pinot-admin script (`pinot-admin.sh`). 

```bash
cd pinot/pinot-tools/target/pinot-tools-pkg
bin/pinot-admin.sh PostQuery \
  -queryType sql \
  -brokerPort 8000 \
  -query "select count(*) from baseballStats"

2020/03/04 12:46:33.459 INFO [PostQueryCommand] [main] Executing command: PostQuery -brokerHost localhost -brokerPort 8000 -queryType sql -query select count(*) from baseballStats
2020/03/04 12:46:33.854 INFO [PostQueryCommand] [main] Result: {"resultTable":{"dataSchema":{"columnDataTypes":["LONG"],"columnNames":["count(*)"]},"rows":[[97889]]},"exceptions":[],"numServersQueried":1,"numServersResponded":1,"numSegmentsQueried":1,"numSegmentsProcessed":1,"numSegmentsMatched":1,"numConsumingSegmentsQueried":0,"numDocsScanned":97889,"numEntriesScannedInFilter":0,"numEntriesScannedPostFilter":0,"numGroupsLimitReached":false,"totalDocs":97889,"timeUsedMs":185,"segmentStatistics":[],"traceInfo":{},"minConsumingFreshnessTimeMs":0}
```

### Pinot clients

Here's a list of the clients available to query Pinot from your application:

- Java client
- Go client
- JDBC client (coming soon)

## What are the key takeaways for Apache Pinot?

- We may not need it at all.
- This isn't our system of record.
