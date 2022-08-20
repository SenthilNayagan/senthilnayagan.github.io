---
layout: post
title:  "Terraform Basics"
kicker: "Terraform"
subtitle: "Terraform is an open-source infrastructure-as-code tool that allows us to programmatically provision the physical resources required for an application to run."
image: assets/images/posts-cover-images/terraform_basics.jpg
author: senthil
published_on: 2022-08-20 00:00:00 +0530
tags: ["terraform", "infrastructure-as-code", "iac"]
categories: terraform
featured: false
hidden: true
---

# Introducing infrastructure as code (IaC)

Infrastructure as Code (IaC) refers to the *process of managing and provisioning infrastructure using code* rather than manual processes. When we talk about "infrastructure as code," we mean that we manage our IT infrastructure with code in the form of configuration files.

With IaC, configuration files are made that describe our infrastructure. This makes it easier to change and share configurations. An IaC process produces the same environment every time it deploys, just as the same source code always generates the same binary.

## It's version control

Version control is an important aspect of IaC, and our configuration files, like any other software source code file, can be under source control.

## The difficulty of manually managing infrastructure

Managing IT infrastructure was traditionally a manual process where deployment team would physically install and configure servers. Unsurprisingly, this manual process would frequently result in a number of issues.

## Uses "declarative" definition files

IaC uses *declarative* definition files, which are simply configuration files. It's worth noting that in the declarative approach, we only specify the "what" but not the "how" in our definition files. A definition file specifies the parts and settings required by an environment, but it does not always specify how to obtain those settings. For example, the definition file may specify the required server version and configuration but not the process for installing and configuring the server—we specify the "what" part but not the "how" part.

## Benefits of infrastructure as code

Because IaC is text-based, we can easily edit, copy, version, and distribute it.

### Speed

The first major benefit of IaC is its speed. We can quickly set up your entire infrastructure by running a script with infrastructure as code. That is something we can do for any environment, from development to production.

### Consistency with reduced errors

Manual processes are prone to errors. Infrastructure as a code solves this problem by making the configuration files the single source of truth. That way, we can be certain that the same configurations will be deployed repeatedly and without error.

### Traceability

We have full traceability of the changes made to each configuration because we can version IaC configuration files like any other source code file, which allows us to save time troubleshooting the problem.

# Introduction to Terraform

Terraform is an open-source *infrastructure as a code* tool from HashiCorp. It enables us to define both cloud and on-premises resources in *human-readable declarative definition files* that can be *versioned*, *reused*, and *shared*. These definition files contain the steps required to provision and maintain our infrastructure. We can edit, review, and version these definition files *just like code*.

## Terraform providers

Terraform makes use of *providers* to connect the Terraform engine to the supported cloud platform. A provider is a *Terraform plugin* that serves as a *translation layer*, allowing Terraform to communicate with a variety of cloud providers, databases, and services.

## Command line interface

We can use the Terraform command line interface (CLI) to manage infrastructure, and interact with Terraform *state*, *providers*, *configuration files*, and *Terraform Cloud*.

## HashiCorp Configuration Language (HCL)

The HashiCorp Configuration Language (HCL) is a one-of-a-kind *configuration language* created by HashiCorp. HCL was created to work with HashiCorp tools, specifically Terraform. The configuration syntax of HCL is simple to read and write. When compared to other well-known configuration languages, such as YAML and JSON, it was designed to have a more clearly visible and defined structure. The file extension of the configuration file written in HCL syntax is `.tf`.

As previously stated, this configuration language is declarative, which means that we only describe the infrastructure we want, and Terraform will figure out how to build it. Terraform can create infrastructure on a variety of cloud platforms such as AWS, Azure, Google Cloud, etc.

## CDK for Terraform

The Cloud Development Kit for Terraform (CDKTF) enables us to define and provision infrastructure using familiar *programming languages*. You might wonder why we need CDKTF when HCL can do the same thing. As mentioned above, CDKTF enables us to define and provision infrastructure using familiar programming languages other than HCL.

At the time of writing this post, Terraform supports the following programming languages:

- Typescript
- Python
- Java
- C#
- Go

### How does CDK for Terraform work?

As a first step, define the infrastructure we want to provision on or more providers using any of the supported programming languages. 

## Download and install

Terraform distribution consists of a single binary file, which can be obtained for free from Hashicorp's [download](https://www.terraform.io/downloads){:target="_blank"} page. There are no dependencies, so we can just copy the executable binary to a folder of our choice and run it from there. HashiCorp also offers a managed solution known as [Terraform Cloud](https://cloud.hashicorp.com/products/terraform){:target="_blank"}.

After we finish the installation, we can run the following command to ensure that everything is working properly:

```shell
$ terraform -v
Terraform v1.2.7
on darwin_amd64
```

## How does Terraform work?

Terraform uses definition files to create and manage resources on cloud or on-premises platforms, as well as other services via their APIs.

### Definition files

Terraform requires infrastructure configuration files written in either HashiCorp Configuration Language (HCL) or JSON syntax. 