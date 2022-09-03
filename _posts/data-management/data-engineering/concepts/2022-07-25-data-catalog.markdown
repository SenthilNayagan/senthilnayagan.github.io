---
layout: post
title:  "Data Catalog"
kicker: "Data Management"
subtitle: "A data catalog is an ordered inventory of an organization's data assets that makes it easy to find the most relevant data quickly."
image: assets/images/posts-cover-images/data-catalog.jpg
author: senthil
date: 2022-07-25 19:30:00 +0530
tags: ["data-engineering", "big-data", "data-lake", "data-catalog", "data-inventory"]
categories: data-management
featured: false
hidden: false
---

> **Writing in progress:** If you have any suggestions for improving the content or notice any inaccuracies, please email me at [hello@senthilnayagan.com](mailto:hello@senthilnayagan.com). Thanks!

# What is a data catalog?
A catalog, in its literal sense, refers to a book or document containing a complete list of things, usually arranged *systematically*. A data catalog, in the context of data management, is an organised inventory of all *data assets* inside an organisation to help data professionals quickly and easily locate the most relevant data with the aim of gaining business insights. 

With a data catalog, the entire organization is given the ability to see all the data it has. Data catalogs have emerged as a powerful tool for data management and data governance.

## What is data assets?

The data assets consist of, but are not limited to:

- Files and folders
- Structured (tabular) data
- Unstructured data
- Reports and query results
- Data visualisations and dashboards
- Machine learning models

## Metadata catalog
A data catalog, at its core, is a *collection of metadata* combined with data management (e.g., access permissions) and search tools. We can also call it a *metadata catalog*. When the data catalog gathers metadata about a data asset, it obtains information such as the asset's creation date, owner, column names, schema name, and so on. Put it simply, the metadata summarises or describes the underlying data assets.

> **Metadata Indexing:** The underlying data assets are not indexed by the data catalog. Only the metadata describing these data assets is indexed.

## Organizes metadata

We mentioned in the definition of a data catalog that it is *an "organized" inventory of all data assets*. When it says "organized," it signifies that the data catalog's metadata is well organized based on domains and other parameters.

A domain is a collection of data assets that are conceptually related. Finance, sales, inventories, and so forth are examples of domains. It is up to the domain owners to specify which data assets get into their domain in a data catalog.

## Data catalog doesn't reveal actual data

The data catalog only provides an overview at the metadata level and does not expose any actual data. As a result, we can let everyone see everything without worry of exposing confidential or sensitive data.

## What kind of queries may be sent to a data catalog?

A data catalog should, at the very least, respond to:
- Where can I get my data?
- Are these data relevant and important?
- What do these data indicates?
- How can I make use of this data?

## What can be accomplished with a data catalog?

Generally, the data catalog gives users access to tools that let them accomplish the following:
- Search the catalog with flexible searching and filtering options
- Data discovery
- Data governance in compliance with organisational or governmental rules

## How does a data catalog get created?

A data catalog can retrieve metadata from our IT landscape (data sources) using a built-in crawler. Crawler is a pull-based system, however data catalog may also be push-based. 

> The data catalogs that crawl the data sources (i.e., that pull, not push) must have a wide variety of connectors to support various data sources.

The IT landscape that is reflected in our data catalog will get business terminology added to it as “tags”. We can also enhance our data catalog with additional information such as roles, supplementary descriptions, classifications, and so on.

A data catalog has various roles built into it, such as data steward, data owner, and others that all perform certain functions in the data catalog.

## Searchable data catalog

Search is one of the most important features of a data catalog. A data catalog enables all data specialists in an organization to search for any data.

> **Data discovery:** Searching and finding relevant data is called data discovery. It determines if the data exists or not, not what is contained inside it.



