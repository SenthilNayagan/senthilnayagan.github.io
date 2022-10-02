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

We can use the metadata from AWS Glue Data Catalog to orchestrate ETL jobs that transform data sources and load our data warehouse or data lake.

## Crawler

A crawler accesses our data store (source or target), extracts metadata, and creates table definitions in a centralized metadata catalog known as the **AWS Glue Data Catalog** for later querying and analysis. The ETL jobs that we define in AWS Glue use these Data Catalog tables as sources and targets.

## Clasifier

A classifier is *part of a crawler* that is triggered during a crawl task. A classifier reads the data in a data store and checks whether a given file is in a format the crawler can handle. If it recognizes the format of the data, it generates a schema in the form of a `StructType` object that corresponds to the structure of the data. AWS Glue provides a set of built-in classifiers, but we can also create custom classifiers.

Classifiers are provided by AWS Glue for a variety of commonly used file formats, including CSV, JSON, AVRO, XML, and others. In addition to this, it offers classifiers for common relational database management systems using a JDBC connection.

## Connection

TODO

## Database

A set of associated Data Catalog table definitions organized into a logical group.

## Table

The schema of our data, which is often referred to as the metadata definition that describes our data, It is important to keep in mind that a table only stores the schema representation of our data, which includes the names of columns, definitions of data types, information on partitions, and other metadata about the actual data. The actual data remains in its original data store, whether it be in a file or a relational database table. 

## Dynamic Frame

A distributed table that supports nested data such as structures and arrays. Each record contains both data and the schema that describes that data. You can use both dynamic frames and Apache Spark DataFrames in our ETL scripts, and convert between them. Dynamic frames provide a set of advanced transformations for data cleaning and ETL.

## Job

The necessary business logic to carry out the tasks associated with ETL. It is composed of a transformation script, data sources, and data targets. Job runs are initiated by triggers that can be scheduled or triggered by events.

## Script

Code that extracts data from sources, transforms it, and loads it into targets. AWS Glue generates PySpark or Scala scripts.

## Trigger

Initiates an ETL job. Triggers can be defined based on a scheduled time or an event.

## Development endpoint

It creates a development environment where the ETL job script can be tested, developed, and debugged.