---
layout: post
title:  "Introduction to Elasticsearch"
kicker: "ELASTICSEARCH"
subtitle: "Elasticsearch is a distributed search and analytics engine."
image: assets/images/posts-cover-images/elasticsearch.jpg
author: senthil
date: 2022-11-14 00:00:00 +0530
tags: ["elasticsearch"]
categories: elasticsearch
featured: false
hidden: true
toc: true
---

# Overview

Elasticsearch is the core of the Elastic Stack. It's a *distributed* search and analytics engine. Logstash and Beats make it easy to collect, combine, enrich, and store our data in Elasticsearch.

# Logstash

Logstash is an open source data collection engine with real-time pipelining capabilities. Logstash can unify data from disparate sources, normalize it in real-time, and send it to destinations of our choice. With a wide range of input, filter, and output plugins, any type of event can be enhanced and transformed. Many native codecs also make the process of ingesting events easier.


## What are Beats?

Beats are lightweight open-source data shippers that we install them as agents on our servers to send specific types of operational data to Elasticsearch.

Elastic provides Beats for capturing:

- Audit data (Auditbeat)
- Log files and journals (Filebeat)
- Cloud data (Functionbeat)
- Availability (Heartbeat)
- Metrics (Metricbeat)
- Network traffic (Packetbeat)
- Windows event logs (Winlogbeat)

Beats can send data directly (or via Logstash) to Elasticsearch. Once the data is in Elasticsearch, it can be further processed and enhanced visualizing it in Kibana. If we want to solve a certain use case, we can create a community Beat.

|![Beats flow](/assets/images/posts/elastic-beats-flow.png)|
|:-:|
|<sup>*Figure 1: Beats flow. Image Courtesy: elastic.co.*</sup>|<br/><br/> 

# Frequently asked questions (FAQ)

## What is the ELK stack?

ELK is an acronym for three open source projects: **Elasticsearch**, **Logstash**, and **Kibana**. 

- **Elasticsearch** is a search and analytics engine. 
- **Logstash** is a server-side data processing pipeline that ingests data from multiple sources simultaneously, transforms it, and then sends it to Elasticsearch. 
- **Kibana** lets users visualize data in Elasticsearch with charts and graphs.

## What is Elastic APM?

APM stands for **application performance monitoring**. APM is one of the most common methods developers use today to measure the *availability*, *response times*, and *behavior of applications and services*.

Elastic APM, on the other hand, is an application performance monitoring system that is built on top of the ELK Stack (**Elasticsearch**, **Logstash**, **Kibana**, and **Beats**). Like other APM solutions, Elastic APM allows us to track key performance related information such as requests, responses, database transactions, errors, etc. Elastic APM ships with support for Java, Go, Node.js, Python, Ruby, .NET, and JavaScript.

The Elastic APM solution is made up of four building blocks:

- Elasticsearch for data storage and indexing
- Kibana for analyzing and visualizing the data
- APM server
- APM agent

APM agents are in charge of gathering performance data and sending it to the APM server. The APM server is in charge of getting the data, turning it into documents, and sending it to Elasticsearch for storage.

|![APM clients and server](/assets/images/posts/elastic-apm.png)|
|:-:|
|<sup>*Figure 1: APM clients and server.*</sup>|<br/><br/>

## When to use Logstash or beats?

Beats are lightweight open-source data shippers that we install them as agents on our servers to send specific types of operational data to Elasticsearch.

Logstash has a larger footprint, but provides a broad array of input, filter, and output plugins for collecting, enriching, and transforming data from a variety of sources.

## What is an inverted index, and why do we need it?