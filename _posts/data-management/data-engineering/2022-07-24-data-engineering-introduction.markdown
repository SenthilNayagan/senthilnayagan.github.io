---
layout: post
title:  "Introduction to Data Engineering"
subtitle: "It's the process of designing and building systems for gathering vast quantities of raw operational data from a variety of sources and formats, analyzing, converting, and storing it at scale."
image: assets/images/posts-cover-images/data-engineering-introduction.jpg
author: senthil
date:   2022-07-24 21:30:00 +0530
tags: ["data-engineering", "big-data", "data-pipeline"]
categories: data-engineering
permalink: /:categories/:title
featured: false
hidden: false
---

# What is data engineering?
Data engineering is the process of designing and building systems for acquiring large amounts of raw operational data from various sources and formats, analysing it, transforming it, and storing it at scale. The management of the data infrastructure is a particular focus of data engineering.

Data is handled by various technologies and stored in a variety of forms and structures, making data analysis difficult. Hence, we need the right specialists (data engineers) and technology to unify various data sets so that they remain available and usable for further analysis by data analysts and scientists.

The first step in data engineering is obtaining vast quantities of data from a variety of sources and bringing them to a data lake or any other big data platform. We call this as *data ingestion*. Once the data has been landed in the data lake, data curation is carried out. Data curation is the process of organising data to fulfil the requirements and interests of a given group or organisation. Here, the data is transformed into value. The diagram below shows all of the different data engineering tasks that happen in the direction shown.

|![Data engineering activities](/assets/images/posts/data-engineering-activities.png)|
|:-:|
|<sub><sup>*Data engineering activities at a high level.*</sup></sub>|

Let's explore each data engineering activity individually.

## Data ingestion
As stated before, data ingestion is the first stage in data engineering. It involves gathering data from many sources and storing it in a data lake or equivalent big data platform. Depending on the business needs, we can use either or both of the following ingestion types to bring in data into the big data platform:
- Real-time ingestion
- Batch ingestion

## What is a data pipeline?

|![Data pipeline](/assets/images/posts/data-pipeline.jpg)|
|:-:|
|[<sub><sup>*Designed by Freepik.*</sup></sub>](https://www.freepik.com/free-vector/pipeline-maintenance-concept-illustration_13717669.htm){:target="_blank"}|

A data pipeline is commonly constructed as part of data engineering to transfer data from a source to a destination. Data is transformed and optimised along the pipeline's journey, eventually reaching a state where it can be analysed and used to generate business insights. Many of the manual processes required in processing and optimising continuous data loads are now automated by modern data pipelines.

Having stated that, data engineering's primary task is the creation of data pipelines. They provide a seamless, automatic flow of data from one stage to another and eliminate the most of manual steps from the process.