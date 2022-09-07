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
hidden: true
toc: true
---

# What is Amazon EMR?

Amazon EMR, formerly known as Amazon Elastic MapReduce, is a *managed cluster platform* that makes it simpler to process and analyze massive volumes of data (big data) on AWS using big data frameworks such as **Apache Hadoop** and **Apache Spark**. Using these big data frameworks and related open-source projects, we can process data for analytics purposes and business intelligence applications.

Using Amazon EMR, we can deploy our workloads (applications) using:

- **Amazon EC2 instance(s)**
- **Amazon Elastic Kubernetes Service (EKS)**
- **On-premises AWS Outposts**

We can run and manage our workloads with the *EMR Console*, *API*, *SDK* or *CLI* and orchestrate them using **Amazon Managed Workflows for Apache Airflow (MWAA)** or **AWS Step Functions**. For an *interactive* experience, **EMR Studio** or **SageMaker Studio** can be used.

Amazon EMR also enables the transformation and movement of huge quantities of data into and out of other AWS data storage and databases, such as **Amazon S3** and **Amazon DynamoDB**.

# Amazon EMR use cases

The following are the use cases:

- Perform big data analytics
- Build scalable data pipelines
- Process real-time data streams
- Accelerate data science and ML adoption

# Getting started with Amazon EMR

## Prerequisite
Before launching an Amazon EMR cluster, the following actions must be completed:

- [**Sign up for AWS**](https://portal.aws.amazon.com/billing/signup){:target="_blank"}
- **Create an Amazon EC2 key pair for SSH into it** - To authenticate and connect to the nodes in a cluster over the SSH protocol, we need to create an EC2 key pair before we launch the cluster.

> **Amazon EC2 key pairs:** A key pair, consisting of a *public key* and a *private key* that we use to prove our identity when connecting to an Amazon EC2 instance. Amazon EC2 stores the public key on our EC2 instance (in an entry within `~/.ssh/authorized_keys` file), and we (client apps) store the private key. Note that Amazon EC2 doesn't keep a copy of our private key.

## Creating Amazon EMR cluster

EMR is built upon a cluster, which is a collection of E2 instances. These instances are generally called *nodes*. There are *different types* of nodes in the cluster, each of which has different roles.

### Amazon EMR node types

There are *three* types of nodes in an EMR cluster:

- **Master node** (At least one master node)
- **Core node** (one or more core nodes)
- **Task node** (optional i.e., zero or more task nodes)

Let's go over each node type in detail.

#### Master node

The master node supervises the cluster and typically runs master application components of distributed applications. For instance, the master node runs the **YARN ResourceManager** service to manage application resources. It also runs the **HDFS NameNode** service, tracks the status of jobs submitted to the cluster, and monitors the health of the instances.

To monitor the progress of a cluster and interact directly with applications, we can connect to the master node over SSH as the Hadoop user. Connecting to the master node enables direct access to directories and files, such as Hadoop log files.

##### Key takeaways

- Master node distribute data and tasks among nodes.
- Tracks the status of tasks.
- Monitors the health of the cluster.

**Multiple master nodes:** With Amazon EMR 5.23.0 or later, it is possible to configure a cluster with *three master nodes* to offer high availability for applications such as YARN Resource Manager, HDFS NameNode, etc. With this capability, the master node is no longer a potential single point of failure.

#### Core nodes

Core nodes are managed by the master node. Core nodes run the **Data Node daemon** to coordinate data storage as part of the **Hadoop Distributed File System (HDFS)**. In addition, they run the **Task Tracker daemon** and perform other parallel processing tasks on data required by installed apps. For example, a core node runs **YARN NodeManager daemons**, **Hadoop MapReduce tasks**, and **Spark executors**.

There is *only one core instance group per cluster*, however the instance group may include many nodes operating on several Amazon EC2 instances. With instance groups, we can add and remove EC2 instances *while the cluster is running*. We can also set up automatic scaling to add instances based on the need.

##### Key takeaways

- Core node runs the actual data processing tasks.
- Stores the data.

#### Task nodes

We can utilize task nodes *to increase the processing capability of parallel computing jobs on data*, such as Hadoop MapReduce tasks and Spark executors. 

> **Note:** Task nodes neither execute the Data Node daemon nor store data in HDFS.

##### Key takeaways

- Task node runs the actual data processing tasks.
- But doesn't store the data.


We've learned about the various types of nodes. Now follow the key steps outlined below to create an EMR cluster:

1. Go to **Services** in the Management Console and select **EMR**. It will take us to the EMR's home screen.
2. Click on **Create cluster**.
3. Click **Go to advanced options** if we want to select specific frameworks or tools as shown in Figure 1 below.
4. Configure **instance group** or **instance fleet** - We specify the configuration of the master, core and task nodes as an instances group or instance fleet.
5. Configure **cluster nodes and instances**.
6. something.

|![Advanced options](/assets/images/posts/amazon-emr-advanced-options-all-frameworks.png "Adanced options")|
|:-:|
|<sup>*Figure 1: Amazon EMR - Adanced options.*</sup>|<br/><br/>

|![Advanced options: Cluster Nodes and Instances](/assets/images/posts/amazon-emr-cluster-nodes-and-instances.png "Adanced options: Cluster Nodes and Instances")|
|:-:|
|<sup>*Figure 2: Amazon EMR - Adanced options: Cluster Nodes and Instances.*</sup>|<br/><br/>

> **Step execution (optional):** A step is a unit of work we submit to the cluster. For instance, a step might contain one or more Hadoop or Spark jobs. We an also submit additional steps to a cluster after it is running. There are various step types: Streming program, Hive program, Spark application, Pig program, and custom JAR.

### Create a cluster with instance fleets or uniform instance groups

When creating an EMR cluster and specifying the configuration of the *master node*, *core nodes*, and *task nodes*, we have *one* of the two available configuration choices:

- **Uniform instance groups**
- **Instance fleets** 

The configuration option we choose is *applicable to all nodes for the lifetime of the cluster*. It's important to note that in a cluster, instance fleets and instance groups *cannot coexist*. We can only choose *one* of these.

#### Instance fleet

The instance fleets configuration offers the widest variety of provisioning choices for Amazon EC2 instances. Each node type (master/core/task node) has a *single instance fleet*, and it is *optional* to use **task instance fleet**. **Up to 5 EC2 instance type** (General purpose instance type, compute optimized instance type, etc.) **per fleet**. If the cluster is created using the AWS CLI or Amazon EMR API, we can have **up to 30 EC2 instance types per fleet**.

<!--
|![Amazon EMR instance fleet](/assets/images/posts/amazon-emr-instance-fleet.png "Amazon EMR instance fleet")|
|:-:|
|<sup>*Figure 3: Amazon EMR - Instance fleet.*</sup>|<br/><br/>
-->

#### Uniform instance groups

Uniform instance groups *offer a simpler setup* than instance fleets. Each Amazon EMR cluster can include **up to 50 instance groups**: 

- One **master instance group** that contains **one** Amazon EC2 instance.
- A **core instance group** that contains **one or more** EC2 instances.
- **Up to 48** optional **task instance groups**.

Each core and task instance group can contain any number of Amazon EC2 instances. We can scale each instance group manually by adding and removing Amazon EC2 instances, or we can set up automatic scaling.

# Frequently asked questions (FAQ)

## What is termination protection?

When termination protection is enabled, the cluster cannot be terminated. Before terminating the cluster, termination protection must first be removed explicitly. This helps ensure that EC2 instances are not accidentally terminated. It *prevents accidental termination* of the cluster. Having said, to shut down the cluster, we must turn off termination protection.

Termination protection is especially helpful if our cluster stores data on local disks that we need to retrieve back before we terminate the instances.

## What is auto-termination?

With auto-termination, we can set a timer to terminate the cluster after a period of inactivity. This would allow us to save cost on unsed instances.

## Archiving Amazon EMR cluster log files

We an configure a cluster to periodically archive the log files stored on the master node to **Amazon S3**. This guarantees that the log files are available after the cluster has terminated, whether normally or as a result of an error. 

Amazon EMR archives the log files to Amazon S3 at *5 minute intervals*. To have the log files archived to Amazon S3, we must enable this feature when we launch the cluster:

|![Amazon EMR - Archiving cluster logs to S3](/assets/images/posts/amazon-emr-archiving-cluster-logs-to-S3.png "Amazon EMR - Archiving cluster logs to S3")|
|:-:|
|<sup>*Figure 3: Amazon EMR - Archiving cluster logs to S3.*</sup>|<br/><br/>