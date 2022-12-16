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
hidden: true
toc: true
---

# Towards user-facing analytics

Real-time analytics have now become something that every business ought to do. It's the process of applying logic to data to get insights or draw conclusions *right away* so that better decisions can be made. "Real time" in real-time analytics means being able to get business insights *as soon as possible* after data (transactions) enters the system. With real-time analytics, businesses can act quickly. They can take advantage of chances and stop problems before they happen.

As opposed to real-time analytics, batch-style analytics may take hours or even days to get results, depending on the volume of data and available computing resources. In batch-style analytics, jobs are generally scheduled to run at night or during non-business hours. So, it often provides us with insights *after the fact*. Most of the time, these batch-type insights are based on stale data (old information), so we can't rely on them. So no one anymore wants to do analytics in batches.

In the world we live in now, everyone needs analytical data, not just business analysts or top-level executives. We call this kind of analytics "**user-facing analytics**." One good example of this is LinkedIn's "Who viewed your profile" feature, which lets all of its more than 700 million users slice and dice the information about who looked at their pages.

In user-facing analytics, users won't put up with painfully slow analytics. When they can find insights in real-time, they are more open to a data-driven culture. So, we need a solution that can work for millions of users and offer fast, real-time insights. Businesses are working hard to speed up the steps needed to get enough data to answer everyone's questions. One such solution that comes to our rescue is "**Apache Pinot**."

|![Evolution of Analytical Data](/assets/images/posts/transactional-data-vs-user-facing-analytical-data.png)|
|:-:|
|<sup>*Figure 1: Evolution of Analytical Data.*</sup>|<br/><br/>|

Pinot joins hands with **Kafka** and **Presto** to provide low-latency, high-throughput user-facing analytics. We'll go through the reasons for using Kafka and Presto with Pinot, and how they complement each other.

# A brief introduction to Apache Pinot

Apache Pinot is a **real-time**, **distributed OLAP datastore** that was built for low-latency, high-throughput analytics, making it perfect for user-facing analytical workloads.

It can ingest directly from streaming data sources like Apache Kafka and Amazon Kinesis and make the events available for querying right away. It can also ingest from batch data sources such as Hadoop HDFS, Amazon S3, Azure ADLS, and Google Cloud Storage.

At the heart of the system is a *columnar store* equipped with advanced indexing and pre-aggregation techniques for low latency. This makes Pinot the best choice for real-time analytics.

One of the best things about Pinot is that it has a pluggable architecture. The plugins make it easy to add support for any third-party system, such as an *execution framework*, a *filesystem*, or an *input format*. For example, there are plugins that make it easy to ingest in data and push it to our Pinot cluster:

- `pinot-batch-ingestion-spark`
- `pinot-s3`
- `pinot-parquet`

|![Apache Pinot Overview](/assets/images/posts/pinot-overview.png)|
|:-:|
|<sup>*Figure 2: Apache Pinot Overview. Image Courtesy: https://docs.pinot.apache.org.*</sup>|<br/><br/>|

# Taking a closer look into Pinot and its components

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
|<sup>*Figure 3: Pinot Cluster's Logical View.*</sup>|<br/><br/>|

- A cluster contains tenants
- Tenants contain tables
- Tables contain segments.

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

Pinot breaks a table into multiple small *chunks of data* known as **segments** and stores these segments in a deep-store such as HDFS as well as Pinot servers. 

For offline tables, segments are built outside of pinot and uploaded using a distributed executor such as Apache Spark or Hadoop. For real-time tables, segments are built in a specific interval inside Pinot.

In the Pinot cluster, a table is modeled as a [**Helix resource**](https://helix.apache.org/Concepts.html){:target="_blank"} and each segment of a table is modeled as a [**Helix Partition**](https://helix.apache.org/Concepts.html){:target="_blank"}.

|![Tenant -> Tables -> Segments](/assets/images/posts/apache-pinot-tenant-table-segment.png)|
|:-:|
|<sup>*Figure 4: Tenant -> Tables -> Segments.*</sup>|<br/><br/>|


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

|![Offline Server](/assets/images/posts/apache-pinot-offline-server-flow.jpg)|
|:-:|
|<sup>*Figure 5: Offline Server. Image Courtesy: https://docs.pinot.apache.org.*</sup>|<br/><br/>|

#### Real-time server

Real-time servers ingest directly from a real-time stream like Kafka. Based on certain thresholds, they make segments of the data that has been stored in-memory from time to time. Then, this segment is saved to the segment store.

|![Real-time Server](/assets/images/posts/apache-pinot-realtime-server-flow.jpg)|
|:-:|
|<sup>*Figure 5: Real-time Server. Image Courtesy: https://docs.pinot.apache.org.*</sup>|<br/><br/>|

### Broker

Broker handles Pinot queries. They accept queries from clients and forward them to the right Pinot servers. They gather results from the servers and combine them into a single response to send back to the client.

|![Broker interaction with other components](/assets/images/posts/apache-pinot-broker-interactions.jpg)|
|:-:|
|<sup>*Figure 6: Broker interaction with other components. Image Courtesy: https://docs.pinot.apache.org.*</sup>|<br/><br/>|

### Controller

The node that observes and controls the participant nodes. In other words, the Pinot controller manages all the components of the Pinot cluster with the help of Apache Helix. It is in charge of coordinating all cluster transitions and making sure that state constraints are met while keeping the cluster stable. **Pinot controllers** are modeled as controllers.

The Pinot controller is really the brains of the cluster, and it takes care of cluster membership, figuring out what data is located on which server, and performing query routing. Pinot controller hosts Apache Helix (for cluster management), and together they are responsible for managing all the other components of the cluster.

The Pinot controller has a user interface that lets us access and query Pinot tables. We can also use this user interface to add, edit, or delete schema and table configurations. We can access the user interface via controller's port (`http://localhost:9000`); port `9000` is the default controller's port.

### Minion

TODO

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

# Pinot's use cases and limitations

## Use cases

Pinot was built to execute real-time OLAP queries on massive amounts of streaming data and events with low latency. Pinot also supports batch use cases with a similar guarantee of low latency. It works well in situations where we need to do quick analytics, such as aggregations, on immutable data that's being received via real-time data ingestion. Also, Pinot is a great choice for querying time series data with lots of dimensions and metrics.

## Limitations

- Pinot is not a replacement for a database and should not be used as a source of truth.
- Pinot is not a replacement for a search engine. 
- Also, Pinot queries cannot span across multiple tables. Table joins and other operations may be accomplished using either the Trino-Pinot connector or the Presto-Pinot connector.

# Getting started with Pinot

## Running Pinot components

Apache Pinot can be run in any of the following environments:

- **locally** on our own computer 
- in **Docker**
- in **Kubernetes**

Here, we'll discuss about [how to deploy and run Apache Pinot locally]({{ site.baseurl }}/apache-pinot/2022/running-apache-pinot-locally){:target="_blank"} on our computer.


## Getting data into Pinot

There are multiple ways for importing data into Pinot. 

To get the data into Pinot, we need a **Pinot schema** and a **Pinot table**. Data ingestion in Pinot involves the following steps:

- Read data and generate compressed segment files from input
- Upload the compressed segment files to output location

Once the location is available to the controller, it can notify the servers to download the segment files and populate the tables. Any distributed executor, such as Hadoop, Spark, Flink, etc., can be used to do the steps above.

> **Note:** Pinot provides runners for Spark out of the box. So we don't have to write a single line of code as users. We can also write runners for any other executor using the provided interfaces.

### Pinot schema

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

#### Schema dimensions

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

### Pinot table

# Frequently asked questions (FAQ)

## What are the challenges that user-facing analytics have to deal with?

- **Latency:** Must be in the sub-second range.
- **Throughput:** Should support millions of concurrent users querying the data.
- **Data freshness:** Insights must be fresh (current), up to date, and relevant.

## What is queries per second (QPS?

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
