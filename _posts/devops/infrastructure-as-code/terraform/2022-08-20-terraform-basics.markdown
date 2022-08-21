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

Each provider adds a set of *resource types* and/or *data sources* that Terraform can manage. Every resource type is implemented by a provider; Terraform cannot manage any infrastructure without providers.

### Where providers come from?

The [Terraform Registry](https://registry.terraform.io/browse/providers){:target="_blank"} is the primary directory of publicly available Terraform providers, hosting providers for the majority of major infrastructure platforms.

### Provider syntax

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

### Terraform CLI commands

Following shows some of the Terraform CLIs:

- `terraform version`
- `terraform init [option]` - This command is used to *initialize a working directory* containing Terraform configuration files and *install the required plugin*. 

## Modules

Modules are small, *reusable Terraform configurations* that allow us to manage a collection of related resources as if they were one.

## State

Terraform must keep track of what infrastructure it created in a `terraform.tfstate` Terraform state file. Terraform uses this state to map real-world resources to our configuration. Terraform state is stored locally by default, but it can also be stored remotely, which is preferable in a team environment.

This state file contains a custom JSON format that records a mapping from the Terraform resources in our templates to their real-world representation.

Terraform makes use of this local state to create plans and modify our infrastructure. Terraform performs a refresh prior to any operation to update the state with the real infrastructure.

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

#### Install CDKTF with npm

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

### CDKTF CLI commands

The following are some of the CDKTF CLI commands:

- `cdktf version`

## How does Terraform work?

As stated above, Terraform uses definition files to create and manage resources on cloud or on-premises platforms, as well as other services via their APIs. Terraform's primary function is to *create*, *modify*, and *destroy* infrastructure resources as described in a Terraform configuration.

### Definition or configuration files

Terraform requires infrastructure configuration or definition files written in either HashiCorp Configuration Language (HCL) or JSON syntax. 

### Working directories

Terraform expects to be invoked from a *working directory* containing Terraform language configuration files. Terraform uses configuration content from this working directory to store *settings*, *cached plugins and modules*, and occasionally *state data*.

#### Working directory contents

A Terraform working directory typically contains:

- A **Terraform configuration** that defines the resources that Terraform should manage. This configuration is likely to change over time.
- A hidden **`.terraform` directory**, which Terraform uses to manage cached provider plugins and modules, record which workspace is currently active, and record the last known backend configuration in case it needs to migrate state on the next run. This directory is automatically managed by Terraform, and is created during initialization.
- **State data**, if the configuration uses the default `local` backend. This is managed by Terraform in a `terraform.tfstate` file (if the directory only uses the default workspace) or a `terraform.tfstate.d` directory (if the directory uses multiple workspaces).

The following shows all the files in a working directory, including the hidden ones:

```shell
$ tree -a

├── .terraform
│   └── providers
│       └── registry.terraform.io
│           └── hashicorp
│               └── local
│                   └── 1.4.0
│                       └── darwin_amd64
│                           └── terraform-provider-local_v1.4.0_x4
├── .terraform.lock.hcl
├── hello.txt
├── main.tf
└── terraform.tfstate
```

### Provisioning infrastructure with Terraform

The provisioning workflow in Terraform is based on the following commands:

- `init`
- `plan`
- `apply`
- `destroy`

#### Init - Initializing working directories

A working directory must be *initialized* before Terraform can perform any operations on it, such as provisioning infrastructure or changing state.

Why do we need to initialize the working directory, and what happens during initialization? The `terraform` binary contains the basic functionality of Terraform, but it does not include the code for any of the providers (e.g., the AWS provider, Azure provider, GCP provider, and so on), so when we first start using Terraform, we must run the `terraform init` command to instruct Terraform to scan the code (configuration file), determine which providers we are using, and download the code from the [Terraform Registry](https://registry.terraform.io/browse/providers){:target="_blank"}. The provider code is downloaded by default into a `.terraform` directory.

> **Note:** It's safe to run init multiple times because the command is idempotent.

After initialization, we will be able to perform other commands, like `terraform plan` and `terraform apply`.

#### Plan

After we've initialized the working directory, we'll use the `plan` command to see what actions Terraform will take to create our resources. This step works similar to the "dry run" feature found in other build systems. This is an excellent way to double-check our changes before releasing them.

The plan command's output is similar to the diff command's output: 

- Resources with a plus sign (`+`) will be created.
- Resources with a minus sign (`-`) will be deleted.
- Resources with a tilde sign (`~`) will be modified in-place.

```shell
$ terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # local_file.hello will be created
  + resource "local_file" "hello" {
      + content              = "Hello, Terraform"
      + directory_permission = "0777"
      + file_permission      = "0777"
      + filename             = "hello.txt"
      + id                   = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

Terraform's `plan` output indicates that it needs to create a new resource, which is expected given that it does not yet exist. We can also see the provided values that we've specified, as well as a pair of permission attributes. Because we did not include them in our resource definition, the provider will use the default values.

#### Apply

We can now use the `apply` command to create actual resources:

```shell
$ terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # local_file.hello will be created
  + resource "local_file" "hello" {
      + content              = "Hello, Terraform"
      + directory_permission = "0777"
      + file_permission      = "0777"
      + filename             = "hello.txt"
      + id                   = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
╷
│ Warning: Version constraints inside provider configuration blocks are deprecated
│
│   on main.tf line 2, in provider "local":
│    2:   version = "~> 1.4"
│
│ Terraform 0.13 and earlier allowed provider version constraints inside the provider configuration block, but that is now deprecated and
│ will be removed in a future version of Terraform. To silence this warning, move the provider version constraint into the
│ required_providers block.
╵

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

local_file.hello: Creating...
local_file.hello: Creation complete after 0s [id=392b5481eae4ab2178340f62b752297f72695d57]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

We can now confirm that the file was created with the desired content:

```shell
$ cat hello.txt
Hello, Terraform
```

Also, after running `terraform apply`, the `terraform.tfstate` file has been created with the following content:

```shell
$ cat terraform.tfstate

{
  "version": 4,
  "terraform_version": "1.2.7",
  "serial": 1,
  "lineage": "bd8530c6-6781-a52a-2c26-dd1164afc7cd",
  "outputs": {},
  "resources": [
    {
      "mode": "managed",
      "type": "local_file",
      "name": "hello",
      "provider": "provider[\"registry.terraform.io/hashicorp/local\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "content": "Hello, Terraform",
            "content_base64": null,
            "directory_permission": "0777",
            "file_permission": "0777",
            "filename": "hello.txt",
            "id": "392b5481eae4ab2178340f62b752297f72695d57",
            "sensitive_content": null
          },
          "sensitive_attributes": [],
          "private": "bnVsbA=="
        }
      ]
    }
  ]
}
```