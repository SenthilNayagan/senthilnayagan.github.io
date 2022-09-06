---
layout: post
title:  "Overview of Amazon EMR"
kicker: "Amazon EMR"
subtitle: "Amazon EMR is a managed cluster platform that makes it easier to run big data frameworks like Apache Hadoop and Apache Spark on AWS to process and analyze huge amounts of data."
image: assets/images/posts-cover-images/amazon-emr.jpg
author: senthil
date: 2022-09-06 00:00:00 +0530
tags: ["aws", "amazon-emr"]
categories: amazon-emr
featured: false
hidden: false
---

# What is Amazon EMR?

Amazon EMR, formerly known as Amazon Elastic MapReduce, is a *managed cluster platform* that makes it simpler to process and analyze massive volumes of data on AWS using big data frameworks such as **Apache Hadoop** and **Apache Spark**. Using these big data frameworks and related open-source projects, we can process data for analytics purposes and business intelligence applications.

Amazon EMR also enables the transformation and movement of huge quantities of data into and out of other AWS data storage and databases, such as **Amazon S3** and **Amazon DynamoDB**.

# Amazon EMR use cases

The following are the use cases:

- Perform big data analytics
- Build scalable data pipelines
- Process real-time data streams
- Accelerate data science and ML adoption

# Getting started with Amazon EMR

Before launching an Amazon EMR cluster, the following actions must be completed:

- [**Sign up for AWS**](https://portal.aws.amazon.com/billing/signup){:target="_blank"}
- **Create an Amazon EC2 key pair for SSH into it** - To authenticate and connect to the nodes in a cluster over the SSH protocol, we need to create an EC2 key pair before we launch the cluster.

> **Amazon EC2 key pairs:** A key pair, consisting of a *public key* and a *private key* that we use to prove our identity when connecting to an Amazon EC2 instance. Amazon EC2 stores the public key on our EC2 instance (in an entry within `~/.ssh/authorized_keys` file), and we (client apps) store the private key. Note that Amazon EC2 doesn't keep a copy of our private key.

# Creating Amazon EMR cluster

Follow the minimal steps below to create an EMR cluster:

1. Go to **Services** in the Management Console and select **EMR**. It will take us to the EMR's home screen.
2. Click on **Create cluster**.
3. Click **Go to advanced options** if we want to select specific frameworks or tools as shown in Figure 1 below.
4. Configure cluster nodes and instances.
5. something.

|![Advanced options](/assets/images/posts/amazon-emr-advanced-options-all-frameworks.png "Adanced options")|
|:-:|
|<sup>*Figure 1: Amazon EMR - Adanced options.*</sup>|<br/><br/>

|![Advanced options: Cluster Nodes and Instances](/assets/images/posts/amazon-emr-cluster-nodes-and-instances.png "Adanced options: Cluster Nodes and Instances")|
|:-:|
|<sup>*Figure 2: Amazon EMR - Adanced options: Cluster Nodes and Instances.*</sup>|<br/><br/>

> **Step execution (optional):** A step is a unit of work we submit to the cluster. For instance, a step might contain one or more Hadoop or Spark jobs. We an also submit additional steps to a cluster after it is running. There are various step types: Streming program, Hive program, Spark application, Pig program, and custom JAR.

## Master node

The master node supervises the cluster and typically runs master application components of distributed applications. For instance, the master node runs the **YARN ResourceManager** service to manage application resources. It also runs the **HDFS NameNode** service, tracks the status of jobs submitted to the cluster, and monitors the health of the instances.

To monitor the progress of a cluster and interact directly with applications, we can connect to the master node over SSH as the Hadoop user. Connecting to the master node enables direct access to directories and files, such as Hadoop log files.

**Multiple master nodes:** With Amazon EMR 5.23.0 or later, it is possible to configure a cluster with *three master nodes* to offer high availability for applications such as YARN Resource Manager, HDFS NameNode, etc. With this capability, the master node is no longer a potential single point of failure.

## Core nodes

Core nodes are managed by the master node. Core nodes run the **Data Node daemon** to coordinate data storage as part of the **Hadoop Distributed File System (HDFS)**. In addition, they run the **Task Tracker daemon** and perform other parallel processing tasks on data required by installed apps. For example, a core node runs **YARN NodeManager daemons**, **Hadoop MapReduce tasks**, and **Spark executors**.

TODO

## Task nodes

TODO