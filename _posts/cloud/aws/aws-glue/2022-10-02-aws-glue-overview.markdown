---
layout: post
title:  "Overview of AWS Glue"
kicker: "AWS Glue"
subtitle: "AWS Glue is a serverless data integration service (commonly known as ETL) that makes it easier to discover, prepare, move, and integrate data from a variety of sources for analytics, machine learning (ML), and application development."
image: assets/images/posts-cover-images/aws-glue.jpg
author: senthil
date: 2022-10-02 00:00:00 +0530
tags: ["aws", "aws-glue", "etl"]
categories: aws-glue
featured: false
hidden: true
toc: true
---

# What is AWS Glue?

AWS Glue is a **"serverless" data integration service** (commonly known as ETL) that makes it easier to *discover*, *prepare*, *move*, and *integrate* data from multiple sources for analytics, machine learning (ML), and application development.

In three significant ways, AWS Glue stands apart from the other ETL tools on the market:

- **Serverless** - We don't need to provision, configure, or spin up any servers, and we don't need to manage the servers' lifecycle.
- **Schema inference** - Glue provides crawlers with automatic schema inference for our structured and semi-structured data sets.
- **Autogen ETL scripts** - In order to save us the trouble of starting from scratch, Glue will automatically generate the scripts required to extract, transform, and load our data from source to target.

## Event-driven ETL

AWS Glue can perform our ETL operations as fresh data arrives. For instance, we can enable AWS Glue to start our ETL operations running as soon as fresh data is made available in Amazon S3.

# AWS Glue components

AWS Glue relies on the interaction of several components to create and manage our ETL workflow.

## AWS Glue Data Catalog

A centralized metadata catalog known as the AWS Glue Data Catalog is where the table definitions (schemas) that are inferred by AWS Glue are stored. After the data has been cataloged, it is immediately available for search and query using Amazon Athena, Amazon EMR, and Amazon Redshift Spectrum.

Each AWS account has one AWS Glue Data Catalog per AWS Region. Each Data Catalog is a highly scalable collection of tables organized into databases. A table is metadata representation of a collection of structured or semi-structured data stored in sources such as Amazon RDS, HDFS, Amazon OpenSearch Service, and others.

The AWS Glue Data Catalog offers a standardized repository in which disparate systems can store and find information for the purpose of maintaining a record of data that is stored in data silos. After that, we can make use of the metadata that is kept in this catalog to query and transform the actual data in a consistent manner across a broad range  of applications.

We can use the metadata from AWS Glue Data Catalog to orchestrate ETL jobs that transform data sources and load our data warehouse or data lake.

For the purpose of controlling access to the tables and databases, we make use of the Data Catalog in conjunction with AWS IAM and Lake Formation. By doing this, we'll be able to give the rest of the organization a safe way to share data while also protecting sensitive information in a very detailed way. 

## Crawler

A crawler accesses our data store (source or target), extracts metadata, and creates table definitions in a centralized metadata catalog known as the **AWS Glue Data Catalog** for later querying and analysis. The ETL jobs that we define in AWS Glue use these Data Catalog tables as sources and targets.

## Clasifier

A classifier is *part of a crawler* that is triggered during a crawl task. A classifier reads the data in a data store and checks whether a given file is in a format the crawler can handle. If it recognizes the format of the data, it generates a schema in the form of a `StructType` object that corresponds to the structure of the data. AWS Glue provides a set of built-in classifiers, but we can also create custom classifiers.

Classifiers are provided by AWS Glue for a variety of commonly used file formats, including CSV, JSON, AVRO, XML, and others. In addition to this, it offers classifiers for common relational database management systems using a JDBC connection.

## Connection

An AWS Glue connection is a Data Catalog object that stores login credentials, URI strings, information about virtual private cloud (VPC), and other relevant data for a particular data store. AWS Glue crawlers, jobs, and development endpoints use connections in order to access certain types of data stores.

AWS Glue supports the following connection types:

- JDBC
- Amazon Relational Database Service (Amazon RDS)
- Amazon Redshift
- Amazon DocumentDB
- Kafka
- MongoDB
- Network (designates a connection to a data source that is in an Amazon VPC)

## Database

A set of associated Data Catalog table definitions organized into a logical group.

## Table

The schema of our data, which is often referred to as the metadata definition that describes our data, It is important to keep in mind that a table only stores the schema representation of our data, which includes the names of columns, definitions of data types, information on partitions, and other metadata about the actual data. The actual data remains in its original data store, whether it be in a file or a relational database table. 

## Dynamic Frame

A distributed table which can handle nested data, such as structures and arrays. Each record includes both the data itself as well as the schema that specifies those data. In our ETL scripts, we are able to use both dynamic frames and Apache Spark DataFrames, as well as convert between the two types. Dynamic frames provide a set of advanced transformations for data cleaning and ETL.

## Job

The necessary business logic to carry out the tasks associated with ETL. It is composed of a transformation script, data sources, and data targets. Job runs are initiated by triggers that can be scheduled or triggered by events.

## Script

Script is the code that reads data from sources, transforms it as necessary, and then loads it into targets. Using the metadata in the Data Catalog, AWS Glue can automatically generate Scala or PySpark scripts with AWS Glue extensions that we can use and modify to perform various ETL operations. 

## Trigger

Initiates an ETL job. Triggers can be defined based on a scheduled time or an event.

## Development endpoint

It creates a development environment where the ETL job script can be tested, developed, and debugged.

## Worker

With AWS Glue, we only pay for the time your ETL job takes to run. There are no resources to manage, no upfront costs, and we are not charged for startup or shutdown time. We are charged an hourly rate based on the number of **Data Processing Units** (or DPUs) used to run our ETL job. A single Data Processing Unit (DPU) is also referred to as a worker. AWS Glue comes with three worker types to help us select the configuration that meets our job latency and cost requirements. Workers come in Standard, G.1X, G.2X, and G.025X configurations.

# Populate data catalog with crawlers

We can use a crawler to populate the AWS Glue Data Catalog with tables. A crawler is capable of crawling multiple data stores in a single run. When it's done, the crawler will either generate new table(s) in our Data Catalog or update the ones it already has. ETL jobs that we define in AWS Glue use these Data Catalog tables as sources and targets.

## Crawler prerequisites

When we create the crawler, the crawler will automatically take on the permissions of the IAM role that we specify. This IAM role must have permissions to extract data from your data store and write to the Data Catalog.

For our crawler, we can create a role and attach the following policies:

- The `AWSGlueServiceRole` AWS managed policy, which grants the required permissions on the Data Catalog.
- An inline policy that grants permissions on the data source.

## Which data stores can we crawl?

Crawlers can crawl the following file-based and table-based data stores:

- Native client
    - Amazon S3
    - Amazon DynamoDB
    - Delta Lake
- JDBC
    - Amazon Redshift
    - Amazon RDS (Amazon Aurora, MariaDB, MS SQL Server, MySQL, Oracle, PostgreSQL)
- MongoDB client
    - MongoDB
    - Amazon DocumentDB (with MongoDB compatibility)

## How crawlers work?

When a crawler runs, it takes the following actions:

- **Classification of data** - Classifies data to determine the format, schema, and associated properties of the raw data.
- **Grouping of data** - Groups data into tables or partitions based on crawler's principles.
- **Writing metadata to the Data Catalog**

When we are defining a crawler, we choose one or more classifiers that analyze the structure of our data in order to infer a schema. AWS Glue has built-in classifiers that can determine schemas based on the files that are commonly used. However, we are also able to create custom classifiers in a separate operation, before we define the crawlers.

The metadata tables that a crawler creates are contained in a database when we define a crawler. If our crawler does not specify a database, our tables are placed in the default database.


# Frequently asked questions (FAQ)

