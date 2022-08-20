---
layout: post
title:  "Terraform Basics"
kicker: "Terraform"
subtitle: "Terraform is an open source infrastructure-as-code tool that allows us to programmatically provision the physical resources required for an application to run."
image: assets/images/posts-cover-images/terraform_basics.jpg
author: senthil
published_on: 2022-08-20 00:00:00 +0530
tags: ["terraform", "infrastructure-as-code", "iac"]
categories: terraform
featured: false
hidden: false
---

> **Writing in progress:** If you have any suggestions for improving the content or notice any inaccuracies, please email me at [hello@senthilnayagan.com](mailto:hello@senthilnayagan.com). Thanks!

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

### Traceability with the help of verison control system

We have full traceability of the changes made to each configuration because we can version IaC configuration files like any other source code file, which allows us to save time troubleshooting the problem.

# Introduction to Terraform

Terraform is an open-source *infrastructure as a code* tool from HashiCorp. It enables us to define both cloud and on-premises resources in *human-readable declarative definition files* (aka configuration files) that can be *versioned*, *reused*, and *shared*. These definition files contain the steps required to provision and maintain our infrastructure. We can edit, review, and version these definition files *just like code*.

Terraform can create infrastructure on a variety of cloud platforms such as AWS, Azure, Google Cloud, etc.

## Download and install

Terraform distribution consists of a single binary file, which can be obtained for free from Hashicorp's [download](https://www.terraform.io/downloads){:target="_blank"} page. There are no dependencies, so we can just copy the executable binary to a folder of our choice and run it from there. HashiCorp also offers a managed solution known as [Terraform Cloud](https://cloud.hashicorp.com/products/terraform){:target="_blank"}.

After we finish the installation, we can run the following command to ensure that everything is working properly:

```shell
$ terraform -v
Terraform v1.2.7
on darwin_amd64
```

## Terraform providers

Terraform makes use of *providers* to connect the Terraform engine to the supported cloud platform. A provider is a *Terraform plugin* that serves as a *translation layer*, allowing Terraform to communicate with a variety of cloud providers, databases, and services.

Terraform's most popular providers, including major cloud providers:

- AWS
- Azure
- Google Cloud Platform
- Kubernetes
- Oracle Cloud Infrastructure

The general syntax is as follows:

```plain
resource "<PROVIDER>_<TYPE>" "<NAME>" {
 [CONFIG...]
}
```

The PROVIDER above is the name of the *prodiver* (e.g., aws). The TYPE is the type of *resource* to create in that provider (e.g., instance, which represents an EC2 instance). The NAME is the *identifier* we can use throughout the Terraform code to refer to this resource. The CONFIG consists of one or more arguments that are specific to that resource (e.g., ami = "ami-1fc93908a9403"). 

For instance, the **aws_instance** resource has many different arguments, but for now, we only need to set the following:

- **`ami`**: The Amazon Machine Image (AMI) to run on the EC2 instance.
- **`instance_type`**: The type of EC2 Instance that will be used. Each EC2 Instance type has a different amount of CPU, memory, disk space, and networking capacity. We uses `t2.micro` instance. T2 instances are available to use in the [AWS Free Tier](http://aws.amazon.com/free){:target="_blank"}, which includes 750 hours of Linux and Windows `t2.micro` instances each month for one year for new AWS customers.

An example of a simple configuration is as follows:

```terraform
resource "aws_instance" "demo" {
  ami = "ami-1fc93908a9403"
  instance_type = "t2.micro"
}
```

## Command line interface

We can use the Terraform command line interface (CLI) to manage infrastructure, and interact with Terraform *state*, *providers*, *configuration files*, and *Terraform Cloud*.

## Modules

Modules are small, *reusable Terraform configurations* that allow us to manage a collection of related resources as if they were one.

## Policy libraries

A library of policies that can be used within Terraform Cloud to accelerate our adoption of *policy as code*.


## HashiCorp Configuration Language (HCL)

The HashiCorp Configuration Language (HCL) is a one-of-a-kind *configuration language* created by HashiCorp. HCL was created to work with HashiCorp tools, specifically Terraform. The Terraform language's primary function is to declare resources, which represent infrastructure artifacts.

### Configuration syntax

HCL is intended to *support multiple syntaxes* for configuration, but the native syntax is the primary format and is optimized for human authoring and maintenance rather than machine generation. The native configuration syntax of HCL is relatively easy for humans to read and write. 

The file extension of the configuration file written in HCL syntax is `.tf`. The Terraform language also allows us to define *resource dependencies*. A configuration can consist of multiple files and directories.

### Declarative configuration

As previously stated, this configuration language is declarative, which means that we only describe the infrastructure we want, and Terraform will figure out how to build it. In other words, it means that we describe our intended goals (what to be done) rather than the steps (how to be done) to achieve those goals.

The configuration files we write in the Terraform configuration language tell Terraform:

- What plugins to install?
- What infrastructure to create?
- What data to fetch?

### Basic configuration elements 

The syntax of the Terraform language consists of only a few basic elements:

- **Blocks** 
- **Arguments**
- **Expressions**

#### Blocks

Block are *containers* for other content, and they typically represent the configuration of an object, such as a *resource*. The block body is delimited by the `{` and `}` characters.

Blocks have:

- A *block type*
- Zero or more *labels*
- A *body* that contains any number of arguments and nested blocks. Top-level blocks in a configuration file control the majority of Terraform's features.

```plain
resource "aws_instance" "demo" {
  ami = "ami-1fc93908a9403"

  network_interface {
    # ...
  }
}
```

In the above example, a block has a *type* as `resource`. The `resource` block type in our case expects two labels: `aws_instance` and `demo`.

The Terraform language uses a limited number of top-level block types, which are blocks that can appear outside of any other block in a configuration file. The majority of Terraform's features, such as resources, input variables, output values, data sources, and so on, are implemented as top-level blocks.

#### Arguments

Arguments assign a *value* to a *name*. They can be found within *blocks*.

```plain
image_id = "abc123"
```

The identifier before the equals sign is the argument *name* (`image_id` in our case), and the expression after the equals sign is the argument's *value* (`abc123` in our case).

#### Expressions

Expressions represent a value in one of two ways: directly or by referencing and combining other values.

## CDK for Terraform (CDKTF)

The Cloud Development Kit for Terraform (CDKTF) enables us to define and provision infrastructure using familiar *programming languages*. You might wonder why we need CDKTF when HCL can do the same thing. As mentioned above, CDKTF enables us to define and provision infrastructure using familiar programming languages other than HCL or JSON syntax.

At the time of writing this post, Terraform supports the following programming languages:

- Typescript
- Python
- Java
- C#
- Go

CDKTF uses the Cloud Development Kit from AWS, which provides a set of language-native frameworks for defining infrastructure, as well as adapters that allow underlying provisioning tools to use those definitions.

### Install CDKTF

We can install CDKTF with `npm` on most operating systems. We can also install CDKTF with Homebrew on MacOS.

#### Install CDKTF with `npm`

To install the most recent stable release of CDKTF, use `npm install` with the `@latest` tag as shown below:

```shell
npm install --global cdktf-cli@latest
```

#### Install CDKTF using Homebrew

We can also use the [Homebrew package manager](https://brew.sh/){:target="_blank"} to install CDKTF on MacOS systems.

```shell
brew install cdktf
```

#### Verify the CDKTF installation

Verify that you have CDKTF installed by running the `cdktf help` command to show the available subcommands.

### How does CDK for Terraform work?

On a high level, we will follow these steps:

- **Step 1 - Define infrastructure:** As a first step, define the infrastructure we want to provision on one or more providers using any of the supported programming languages. CDKTF works by translating configurations defined in an imperative programming language to JSON configuration files for Terraform.

- **Step 2 - Deploy:** Use `cdktf` CLI commands to provision infrastructure with Terraform or synthesize (translate to other form) our code into a JSON configuration file that others can use with Terraform directly.

#### CDKTF to HCL

The `cdktf synth` command synthesizes (generate of code/configuration) Terraform configuration for the given app. CDKTF stores the synthesized configuration in the `cdktf.out` directory, unless you use the `--output` flag to specify a different location.

The output folder is ephemeral (short lived) and might be erased for each `synth` that we run manually or that happens automatically when we run `deploy`, `diff`, or `destroy`.

#### HCL to CDKTF

Use the `cdktf convert` command to translate Terraform configuration written in HCL to the equivalent configuration in our preferred language. 

Convert a local file:

```shell
cat main.tf | cdktf convert > imported.ts
```

## How does Terraform work?

As stated above, Terraform uses definition files to create and manage resources on cloud or on-premises platforms, as well as other services via their APIs.

### Definition or configuration files

Terraform requires infrastructure configuration or definition files written in either HashiCorp Configuration Language (HCL) or JSON syntax. 