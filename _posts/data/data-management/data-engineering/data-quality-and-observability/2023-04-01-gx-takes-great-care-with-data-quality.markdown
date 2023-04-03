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

Under the hood, Datasources uses a **Data Connector** and an **Execution Engine** to connect to a wide variety of external data sources and perform computation, respectively. The Datasource provides an interface for a Data connector and an Execution Engine to work together. Each Datasource must have an Execution Engine and one or more Data Connectors configured. Thanks to the unified Datasource API, once a Datasource is configured, we will be able to operate with the Datasource's API rather than needing a different API for each data source we may be working with.

### Data Connector

Datasource leverages the Data Connector, which facilitates access to external data sources such as databases, filesystems, and cloud storage. A Data Connector is an integral element of a Datasource. 

### Ececution Engine

Execution Engine provides computing resources that will be used to perform Validation. Great Expectations can take advantage of different Execution Engines, such as Pandas, Spark, or SqlAlchemy.

|![Figure 1: Datasource - How it works?](/assets/images/posts/gx-datasource-how-it-works.png "Created by Author"){: width="80%" }|
|:-:|
|<sup>*Figure 1: Datasource - How it works?*</sup>|<br/><br/>

## Data Asset

A Data Asset is a *logical* collection of records within a Datasource. Often, Data Assets are tied to already-existing data that has a name (e.g., "the UserEvents table"). **Also, Data Assets can slice the data one step further** (subsets) (e.g., “new records for month within the UserEvents table.”). Great Expectations protects the quality of Data Assets.

More Data Asset examples are:

- In a SQL database, a Data Asset may be the rows from a table grouped by the week.
- In an S3 bucket or filesystem, a Data Asset may be the files matching a particular regex pattern.

We can define multiple Data Assets built from the same underlying data source to support different workflows or use cases. To put it simply, the same data can be in multiple Data Assets. For instance, we may have different **Expectations** of the same raw data for different purposes.

Not all records in a Data Asset need to be available at the same time or same place. A Data Asset could be built from:

- Streaming data that is never stored
- Incremental deliveries
- Incremental updates
- Analytic queries
- Replacement deliveries or from a one-time snapshot

That implies that a Data Asset is a logical concept. So no matter where the data comes from originally, Great Expectations validates **batches** of data.

## Batch

A Batch is a discrete selection or subset of records from a Data Asset. Providing a **Batch Request** to a Datasource results in the creation of a Batch. **A Batch adds metadata to precisely identify the specific data included in the Batch**. With the help of these metadata, a Batch can be identified by a collection of parameters, such as *the date of delivery*, *the value of a field*, *the time of validation*, or *access control permissions*. 

## Batch Request

A Batch Request is sent to a **Datasource** in order to create a **Batch**. A Batch Request contains all the necessary details to query the underlying data. A Batch Request will return all matching Batches if it finds more than one that satisfy the requirements of the user-provided `batch identifiers`. A Batch Request is always used when Great Expectations builds a Batch.

When a Batch Request is passed to a Datasource, the Datasource will use its Data Connector to build a **Batch Spec**, which is an Execution Engine-specific description of the Batch. Datasource's Execution Engine will use this Batch Spec to return a Batch of data.

We will rarely need to access an existing Batch Request. Instead, we often find ourself defining a Batch Request in a configuration file, or passing in parameters to create a Batch Request which we will then pass to a Datasource. Once we receive a Batch back, it is unlikely we will ever need to reference to the Batch Request that generated it. In fact, if the Batch Request was part of a configuration, Great Expectations will simply initialize a new copy rather than load an existing one when the Batch Request is needed.

### How to create a Bad Request

Batch Requests are instances of either a `RuntimeBatchRequest` or a `BatchRequest`. A BatchRequest can be defined by passing a dictionary with the necessary parameters when a BatchRequest is initialized.

```python
from great_expectations.core.batch import BatchRequest

batch_request_parameters = {
  'datasource_name': 'getting_started_datasource',
  'data_connector_name': 'default_inferred_data_connector_name',
  'data_asset_name': 'yellow_tripdata_sample_2019-01.csv',
  'limit': 1000
}

batch_request=BatchRequest(**batch_request_parameters)
```

## Data Context

A Data Context is the **primary entry point for a Great Expectations**. Our Data Context provides us with methods to configure our Stores, plugins, and Data Docs. It also provides the methods needed to create, configure, and access our Datasources, Expectations, Profilers, and Checkpoints. In addition to all of that, it internally manages our Metrics, Validation Results, and the contents of your Data Docs for us. Expectations, Profilers, Checkpoints, Metrics, and Validation Results will all be covered in greater depth later on.

## Expectation

An Expectation is a **test assertion that we can run against our data under test**. Like unit test assertions in most of the programming languages, Expectations provide a flexible, declarative language for describing expected behavior. Unlike traditional unit tests, **Great Expectations applies Expectations to data instead of code**. As an example, we could define an Expectation that states that a column has no null values. Great Expectations would then compare our data to that Expectation and report if a null value was found.

## Expectation Suite

Expectations are grouped into Expectation Suites, which can be stored and retrieved using an **Expectation Store**. The most critical aspect of Great Expectation is creating Expectation, or Expectation Suites. Note that a local configuration for an Expectation Store will be added automatically to `great_expectations.yml` when we initialize our Data Context for the first time. We can change this configuration to work with different **Stores**.

Generally, we will not need to interact with an Expectation Store directly. Instead, our Data Context will use an Expectation Store to store and retrieve Expectation Suites behind the scenes. This means, we most likely use convenience methods in our Data Context to retrieve Expectation Suites.

## Expectation Store

Expectation Stores allow us to store and retrieve Expectation Suites. These Stores can be accessed and configured through the Data Context, but entries are added to them when we save an Expectation Suite.

## Store

A Store is a connector to store and retrieve information about metadata in Great Expectations. Great Expectations supports a variety of Stores for different purposes, but the most common Stores are: 

- **Expectation Stores** - Used to store and retrieve information about collections of test assertions about data.
- **Validations Stores** - Used to store and retrieve information about objects generated when data is Validated against an Expectation Suite.
- **Checkpoint Stores** - 
- **Metric Stores** 
- **Evaluation Parameter Stores**
- **Data Docs Stores**

## Checkpoint

In a production deployment of Great Expectations, a Checkpoint serves as the primary means for validating data. Checkpoints provide a convenient abstraction for bundling the Validation of a Batch (or Batches) of data against an Expectation Suite (or several), as well as the Actions that should be taken after the validation.

Checkpoints have their own Store which is used to persist their configurations to YAML files. These configurations can be committed to version control.

A Checkpoint uses a **Validator** to run one or more Expectation Suites against one or more Batches provided by one or more Batch Requests. Running a Checkpoint produces Validation Results and will result in optional Actions being performed if they are configured to do so.


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

