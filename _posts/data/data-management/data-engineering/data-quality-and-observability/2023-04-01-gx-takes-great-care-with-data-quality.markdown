---
layout: post
title:  "Great Expectations takes great care of your data quality"
kicker: "Data Quality"
subtitle: "Information is only valuable if it is of high quality. It's focused on making sure that the data complies with our data quality dimensions."
image: assets/images/posts-cover-images/data-quality-oss-tools.jpg
imageshadow: true
toc: true
author: senthil
date: 2023-04-01 00:000:00 +0530
tags: [ "data-quality", "data", "data-observability", "great-expectations", "gx" ]
categories: data-quality
featured: false
hidden: true
---

# Data quality

Information is only valuable if it is of high quality. How can we get data of such high quality, then?  The answer is simple: testing the data quality is what assures high quality. We know the "what" part of it gets us quality, but the "how" part is much more crucial in producing high-quality data.

## Data quality dimensions

Data quality focuses on ensuring that the data conforms to the **six data quality dimensions** listed below. These data quality dimensions are useful guidelines for enhancing the quality of data assets.

- **Accuracy** - What degree of fact does a piece of information have?
- **Completeness** - The state of being complete and entire.
- **Consistency** - Does information stored in one place match relevant data stored elsewhere?
- **Timeliness** - Is our information available when it's needed?
- **Validity** - Invalid data affects the accuracy: Is information is a certain format, do business standards or rules apply to the information, or is it in an unusable format?
- **Uniqueness** - Ensures duplicate or overlapping data is identified and removed.

## Testing data

Similar to unit testing in software engineering, data testing has to become a regular practice in data engineering. A data acceptance procedure may be established by the organization, according to which data cannot be utilized unless its owners provide evidence that it satisfies the organization's quality standards.

### Data quality testing stages

1. **First stage:** The first place that we need to test data is **at the point of ingestion**. When ingesting data, we want to be sure that all of the data has successfully moved from its source to the target destination.
2. **Second stage:** The second place that we need to test data is **at the point of transformation**. With transformation testing, we will typically check pre- and post-conditions, such as:
   - Null checks
   - Valid value checks
   - Schema checks
   - Referential integrity checks, and so on.

### Data quality tools

There are various data quality tools—both commercial and open source—that are currently on the market. This blog focuses on one of the hand-picked open source data quality tools called **Great Expectations**, among other open source tools

I took into account the following factors while evaluating the open source data quality tools:

- Is the tool **able to deal with all six data quality dimensions**?
- **How easy is it for both data engineers and data analysts** to learn how to write data quality checks or tests and get good at them quickly?
- **How well the documentation and API guides** are supported?
- **How flexible and extensible** the tool is
- **How active and responsive is the respective community in Slack**? In general, Slack is considered to be far more helpful than submitting our questions elsewhere. We could ask questions and receive straight answers from committers, which is one of the best things about Slack.
- The **rate at which the tool is evolving and gaining new capabilities**.
- Last but not least, which one of them is **more widely used across enterprises**?

Let's take a closer look at Great Expectations to see how it might assist us in obtaining reliable data.

# Introduction to Great Expectations (GX)

Great Expectations, shortly referred to as **GX**, is a **powerful** and **flexible** open-source data quality solution on the market today. We'll take a close look at it with examples to demonstrate how powerful and flexible GX tool is. 

Unlike traditional unit tests, GX applies tests to data instead of code. To put it simply, **in GX, testing is performed on data rather than code**. It's a Python library that enables us to verify that our data is accurate by **validating**, **documenting**, and **profiling** it. 

> **Profiling**, or **data profiling**, is the process of examining, analyzing, and creating useful summaries of data that aid in the discovery of data quality issues.

Before we continue, it's crucial to understand the various concepts and terms used in Great Expectations.

## Datasource

GX provides better connectivity with a wide variety of data sources and data manipulation frameworks like Apache Spark and Pandas. It provides a **unified Datasource API** that *connects* and *interacts* across multiple data sources. The term "unified" denotes that the Datasource API remains the same across all data sources, such as PostgreSQL, CSV filesystems, and others. This unified Datasource API makes working with all data sources very convenient. Having said that, our primary tool for connecting to data is the Datasource.

Under the hood, Datasources uses a **Data Connector** and an **Execution Engine** to connect to a wide variety of external data sources and perform computation, respectively. The Datasource provides an interface for a Data connector and an Execution Engine to work together. Each Datasource must have an Execution Engine and one or more Data Connectors configured. Thanks to the unified Datasource API,once a Datasource is configured, we will be able to operate with the Datasource's API rather than needing a different API for each possible data backend we may be working with.

### Data Connector

Datasource leverages the Data Connector, which facilitates access to external data sources such as databases, filesystems, and cloud storage. A Data Connector is an integral element of a Datasource. 

### Ececution Engine

Execution Engine provides computing resources that will be used to perform Validation. Great Expectations can take advantage of different Execution Engines, such as Pandas, Spark, or SqlAlchemy.

|![Figure 1: Datasource - How it works?](/assets/images/posts/gx-datasource-how-it-works.png "Created by Author"){: width="80%" }|
|:-:|
|<sup>*Figure 1: Datasource - How it works?*</sup>|<br/><br/>

## Data Asset

A Data Asset is a collection of records within a Datasource. Often, Data Assets are tied to already-existing data that has a name (e.g., "the UserEvents table"). **Data Assets slice the data one step further** (e.g., “new records for each day within the UserEvents table.”). 

Further Data Asset examples are:

- In a SQL database, a Data Asset may be the rows from a table grouped by the week.
- In an S3 bucket or filesystem, a Data Asset may be the files matching a particular regex pattern.

We can define multiple Data Assets built from the same underlying data source to support different workflows.

# Great Expectations in detail

## Installing GX OSS

The following steps show how to install GX OSS (open source software) locally on our desktop.

### Prerequisites

- A supported version of **Python** (versions 3.7 to 3.10)
- The ability to install Python packages with **pip**

Install GX using pip as shown below:

```bash
python -m pip install great_expectations
```

GX and its associated dependencies will be installed by `pip` when we run the above command from the terminal. This may take a moment to complete. For best practices, set up a virtual workspace for our project.

Let's get into the fundamentals of writing test cases, validating them against the data, and generating test reports.

