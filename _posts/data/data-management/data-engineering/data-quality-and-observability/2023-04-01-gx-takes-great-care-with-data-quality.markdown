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

I've taken into account the following factors while evaluating the open source data quality tools:

- Is the tool **able to deal with all six data quality dimensions**?
- **How easy is it for both data engineers and data analysts** to learn how to write data quality checks or tests and get good at them quickly?
- **How well the documentation and API guides** are supported?
- **How flexible and extensible** the tool is
- **How active and responsive is the respective community in Slack**? In general, Slack is considered to be far more helpful than submitting our questions elsewhere. We could ask questions and receive straight answers from committers, which is one of the best things about Slack.
- The **rate at which the tool is evolving and gaining new capabilities**.
- Last but not least, which one of them is **more widely used across enterprises**?

Let's take a closer look at **Great Expectations** to see how it might assist us in obtaining reliable data.

# Introduction to Great Expectations (GX)

Great Expectations, shortly referred to as **GX**, is a **powerful** and **flexible** open-source data quality solution on the market today. We'll take a close look at it with examples to demonstrate how powerful and flexible GX tool is. 

Unlike traditional unit tests, GX applies tests to data instead of code. To put it simply, **in GX, testing is performed on data rather than code**. It's a Python library that enables us to verify that our data is accurate by **validating**, **documenting**, and **profiling** it. 

> **Profiling**, or **data profiling**, is the process of examining, analyzing, and creating useful summaries of data that aid in the discovery of data quality issues.

The best part about Great Expectations is that, unlike other data quality tools, **we do not need to write the configuration**. Instead, Great Expectations comes with the Jupyter Notebook, which will help us generate various configurations for us.

At a high level, there are four stages in Great Expectations:

1. **Setup**
2. **Connect to Data**
3. **Create Expectations**
4. **Validate Data**

|![Figure 1: Four stages of Great Expectations](/assets/images/posts/gx-four-stages.png "Created by Author"){: width="90%" }|
|:-:|
|<sup>*Figure 1: Four stages of Great Expectations.*</sup>|<br/><br/>

Various activities go into each stage as shown below. We will go into great detail on each of these activities later on. For now, it's crucial to understand the various concepts and terms used in Great Expectations. 

|![Figure 2: Various activities go into each stage](/assets/images/posts/gx-stages-and-activities.png "Created by Author")|
|:-:|
|<sup>*Figure 2: Various activities go into each stage.*</sup>|<br/><br/>


## Data Context

A Data Context is the **primary entry point for a Great Expectations**. Our Data Context provides us with methods to configure our Stores, plugins, and Data Docs. It also provides the methods needed to create, configure, and access our Datasources, Expectations, Profilers, and Checkpoints. In addition to all of that, it internally manages our Metrics, Validation Results, and the contents of your Data Docs for us. Expectations, Profilers, Checkpoints, Metrics, and Validation Results will all be covered in greater depth later on.

Data Context can be initialized using the CLI, created, loaded, and saved for future use.

### Initialize our Data Context with the CLI

The simplest way to create a new Data Context is by using Great Expectations' CLI. Run the following command from the directory where we wish to initialize Great Expectations:

```bash
great_expectations init
```

The above command causes Great Expectations to create the directory structure and configuration files required for us to go on. Great Expectations will create a new directory with the following structure:

```text
great_expectations
    |-- great_expectations.yml
    |-- expectations
    |-- checkpoints
    |-- plugins
    |-- .gitignore
    |-- uncommitted
        |-- config_variables.yml
        |-- data_docs
        |-- validations
```

### Create our Data Context

Use the following Python statement to create a new Data Context:

```python
import great_expectations as gx

context = gx.get_context()  # Creating a DataContext object

# The below statement is the same as above with a variable-type annotation. 
# It's a more clean way of coding.
# context: gx.DataContext = gx.get_context()
```

### Load our Data Context

Load an on-disk Data Context via:

```python
import great_expectations as gx

context = gx.get_context(
    context_root_dir='path/to/my/context/root/directory/great_expectations'
)
```

### Save the Data Context for future use

We obtained a temporary, in-memory **Ephemeral Data Context** from `gx.get context()` since we had not previously initialized a Filesystem Data Context (using `great_expectations init`) or specified a path at which to create one (via `gx.get_context(context_root_dir='path/great_expectations')`).

To save this Data Context for future use, we will convert it to a Filesystem Data Context:

```python
context = context.convert_to_file_context()
```

We can also provide the path to a specific folder where we want the Filesystem Data Context to be initialized.

## Datasource

GX provides better connectivity with a wide variety of data sources and data manipulation frameworks like Apache Spark and Pandas. It provides a **unified Datasource API** that *connects* and *interacts* across multiple data sources. The term "unified" denotes that the Datasource API remains the same across all data sources, such as PostgreSQL, CSV filesystems, and others. This unified Datasource API makes working with all data sources very convenient. Having said that, our primary tool for connecting to data is the Datasource.

Under the hood, Datasources uses a **Data Connector** and an **Execution Engine** to connect to a wide variety of external data sources and perform computation, respectively. The Datasource provides an interface for a Data connector and an Execution Engine to work together. Each Datasource must have an Execution Engine and one or more Data Connectors configured. 

Thanks to the unified Datasource API, once a Datasource is configured, we will be able to operate with the Datasource's API rather than needing a different API for each data source we may be working with.

### Data Connector

Datasource leverages the Data Connector, which facilitates access to external data sources such as databases, filesystems, and cloud storage. A Data Connector is an integral element of a Datasource. 

### Execution Engine

Execution Engine provides computing resources that will be used to perform Validation. Great Expectations can take advantage of different Execution Engines, such as **Pandas**, **Spark**, or **SqlAlchemy**.

Various Execution Engine's class names are listed below. We will discuss in the later section where we will use these Execution Engine classes.

- **Pandas** - `PandasExecutionEngine`
- **Spark** - `SparkDFExecutionEngine`
- **SqlAlchemy** - `SqlAlchemyExecutionEngine`

The following shows the high-level workflow:

|![Figure 3: Datasource - How it works?](/assets/images/posts/gx-datasource-how-it-works.png "Created by Author"){: width="80%" }|
|:-:|
|<sup>*Figure 3: Datasource - How it works?*</sup>|<br/><br/>

### Create a new Datasource through the CLI

Run the below command to create a new Datasource:

```bash
great_expectations datasource new
```

The above command will bring up the following prompt:

```text
What data would you like Great Expectations to connect to?
    1. Files on a filesystem (for processing with Pandas or Spark)
    2. Relational database (SQL)
: 1
```

We can get data either: 

- From the filesystem (a file-based Datasource) using Pandas or Spark
- From a relational database

#### File-based Datasource

For file-based Datasource, the configuration contains an `InferredAssetFilesystemDataConnector`, which will add a **Data Asset** for each file in the base directory we provided. It also contains a `RuntimeDataConnector`, which can accept file paths. Note that we can customize it as we wish.

In the case of Spark as the processing engine, the following is the Datasource configuration (as a Python string):

```python
datasource_yaml = f"""
name: {"my_datasource"}
class_name: Datasource
execution_engine:
  class_name: SparkDFExecutionEngine
data_connectors:
  default_inferred_data_connector_name:
    class_name: InferredAssetFilesystemDataConnector
    base_directory: ../data
    default_regex:
      group_names:
        - data_asset_name
      pattern: (.*)
  default_runtime_data_connector_name:
    class_name: RuntimeDataConnector
    assets:
      my_runtime_asset_name:
        batch_identifiers:
          - runtime_batch_identifier_name
"""
```

### Test our Datasource configuration

Use `context.test_yaml_config(...)` to test our Datasource configuration as shown below:

```python
context.test_yaml_config(yaml_config=datasource_yaml)
```

In the above Python statement, `context` is the Data Context object, which can be created as follows:

```python
import great_expectations as gx

context = gx.get_context()

# The below statement is the same as above with a variable-type annotation. 
# It's a more clean way of coding.
# context: gx.DataContext = gx.get_context()
```

### Save our Datasource configuration

Here we save our Datasource in our Data Context once we are satisfied with the configuration using the following Python statement:

```python
from great_expectations.cli.datasource import sanitize_yaml_and_save_datasource

sanitize_yaml_and_save_datasource(context, example_yaml, overwrite_existing=False)
```
Note that `overwrite_existing` defaults to False, but we can change it to True if we wish to overwrite the configuration. Please note that if we wish to include comments we must add them directly to our `great_expectations.yml`.

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

### How to create a Batch Request

Batch Requests are instances of either a `RuntimeBatchRequest` or a `BatchRequest`. A BatchRequest can be defined by passing a dictionary with the necessary parameters when a BatchRequest is initialized.

```python
from great_expectations.core.batch import BatchRequest

batch_request_parameters = {
  'datasource_name': 'my_datasource',
  'data_connector_name': 'default_inferred_data_connector_name',
  'data_asset_name': 'my-data-under-test.csv',
  'limit': 1000  # Optional
}

batch_request = BatchRequest(**batch_request_parameters)
```

## Expectation

An Expectation is a **test assertion that we can run against our data under test**. Like unit test assertions in most of the programming languages, Expectations provide a flexible, *declarative language* for describing expected behavior. Unlike traditional unit tests, **Great Expectations applies Expectations to data instead of code**. 

Each Expectation is a declarative test assertion about the expected format, content, or behavior of our data under test. The **test assertions are both human-readable and machine-verifiable assertions**. 

> **Expectation Gallery:** Great Expectations comes with many [built-in](https://greatexpectations.io/expectations/){:target="_blank"} Expectations, and we can also develop our own custom Expectations.

As an example, we could define an Expectation that states that a column has no null values. Great Expectations would then compare our data to that Expectation and report if a null value was found.

### Various ways of creating Expectations

There are a few workflows we can follow when creating Expectations. These workflows represent various ways of creating Expectations.

There are **four** potential ways to create Expectations as shown below:

1. **Interactive workflow** (with inspecting data) (Recommended)
2. **Data Assistant workflow** (with inspecting data) (Recommended)
3. **Manually define our Expectations** (without inspecting data)
4. **Custom scripts**

#### Interactive workflow

In this workflow, we will be working in a Python interpreter or Jupyter Notebook. In this case, we will navigate to our Data Context's root directory in our terminal, where we will use the CLI to launch a Jupyter Notebook, which will assist us in the process. We will use a Validator and call expectations methods on it to define Expectations in an Expectation Suite. When we have finished, we will save that Expectation Suite in our Expectation Store.

Use the CLI to begin the interactive process of creating Expectations. From the root folder of our Data Context, enter the following command:

```bash
great_expectations suite new
```

This will bring up the following prompt:

```text
How would you like to create your Expectation Suite?
    1. Manually, without interacting with a sample Batch of data (default)
    2. Interactively, with a sample Batch of data
    3. Automatically, using a Data Assistant
:
```

To start the Interactive Mode workflow, enter 2. Note that we can skip the above prompt by including the flag `--interactive` in our command-line input:

```bash
great_expectations suite new --interactive
```

#### Data Assistant workflow

In this workflow, we will use a Data Assistant to generate Expectations based on some input data. In this case, we will be working in a Python environment, so we will need to load or create our Data Context as an instantiated object. Next, we will create a Batch Request to specify the data we would like to Profile with our Data Assistant. 

##### Create a Batch Request
This is how we create a `BatchRequest`:

```python
from great_expectations.core.batch import BatchRequest

batch_request_parameters = {
  'datasource_name': 'my_datasource',
  'data_connector_name': 'default_inferred_data_connector_name',
  'data_asset_name': 'my-data-under-test.csv',
  'limit': 1000  # Optional
}

batch_request = BatchRequest(**batch_request_parameters)
```

> **Caution:** The Onboarding Data Assistant will run a high volume of queries against our Datasource. Data Assistant performance can vary significantly depending on the number of Batches, count of records per Batch, and network latency. It is recommended that we start with a smaller BatchRequest if we find that Data Assistant runtimes are too long.

##### Run the Onboarding Data Assistant

Next, run the Onboarding Data Assistant. Running a Data Assistant is as simple as calling the `run(...)` method for the appropriate assistant. There are numerous parameters available for the `run(...)` method of the Onboarding Data Assistant. For instance, the `exclude_column_names` parameter allows us to provide a list columns that should not be Profiled. In addition, we can also use other parameters, such as `include_column_names`, `include_column_name_suffixes`, and `cardinality_limit_mode`.

The following code shows how to run the Onboarding Assistant.

```python
data_assistant_result = context.assistants.onboarding.run(
    batch_request = multi_batch_all_years_batch_request,
    exclude_column_names = [col1, col2, col3],
)
```

##### Save our Expectation Suite

Once we have executed the Onboarding Data Assistant's `run(...)` method and generated Expectations for our data, we need to load them into our Expectation Suite and save them. We will do this by using the Data Assistant result:

```python
expectation_suite = data_assistant_result.get_expectation_suite(
    expectation_suite_name=expectation_suite_name
)
```

And once the Expectation Suite has been retrieved from the Data Assistant result, we can save it like so:

```python
context.add_or_update_expectation_suite(expectation_suite=expectation_suite)
```

#### Manually define our Expectations

This workflow is for advanced users who want to manually (**without inspecting data**) define the Expectations by writing the configurations. While source data is not necessary for this approach, a thorough understanding of the Expectations configurations is necessary.

Create Expectation Configurations as shown below:

```python
expectation_configuration = ExpectationConfiguration(
   expectation_type = "expect_column_values_to_be_in_set",
   kwargs = {
      "column": "transaction_type",
      "value_set": ["purchase", "refund", "upgrade"]
   },
)
```

Then, add the Expectation to the suite as shown below:

```python
suite.add_expectation(expectation_configuration = expectation_configuration)
```

#### Custom scripts

Some advanced users have also taken advantage of this workflow, and have written custom methods that allow them to generate Expectations based on the metadata associated with their source data systems.

## Expectation Suite

**Expectations are grouped into Expectation Suites**, which can be stored and retrieved using an **Expectation Store**. The most critical aspect of Great Expectation is creating Expectation, or Expectation Suites. Note that a local configuration for an Expectation Store will be added automatically to `great_expectations.yml` when we initialize our Data Context for the first time. We can change this configuration to work with different **Stores**.

Generally, we will not need to interact with an Expectation Store directly. Instead, our Data Context will use an Expectation Store to store and retrieve Expectation Suites behind the scenes. This means, we most likely use convenience methods in our Data Context to retrieve Expectation Suites.

### Create a new Expectations Suite

The below shows how to create an Expectations Suite using the CLI. Run the below command from Data Context:

```bash
great_expectations suite new
```

The above command will bring up the following prompt:

```text
How would you like to create your Expectation Suite?
    1. Manually, without interacting with a sample Batch of data (default)
    2. Interactively, with a sample Batch of data
    3. Automatically, using a Data Assistant
```

## Store

A Store is a **location to store and retrieve information about metadata** in Great Expectations. Great Expectations supports a variety of Stores for different purposes, but the most common Stores are: 

- **Expectation Stores** - Used to store and retrieve information about collections of test assertions about data.
- **Validations Stores** - Used to store and retrieve information about objects generated when data is Validated against an Expectation Suite.
- **Checkpoint Stores** - 
- **Metric Stores** 
- **Evaluation Parameter Stores**
- **Data Docs Stores**

### Expectation Store

Expectation Stores allow us **to store and retrieve Expectation Suites**. These Stores can be accessed and configured through the Data Context, but entries are added to them when we save an Expectation Suite.

## Validator

A Validator is the object responsible **for running an Expectation Suite against data**. In other words, we use a Validator to access and interact with your data. Checkpoints, in particular, use Validators when running an Expectation Suite against a Batch Request. However, we can also use our Data Context to get a Validator to use outside a Checkpoint - for instance, to create Expectations interactively in a Jupyter Notebook.

Also, we can use the Validator to verify our Datasource. To verify a new Datasource, we can load data from it into a Validator using a Batch Request.

Note that **Validators don't require additional configuration**. Provide one with an Expectation Suite and a Batch Request, and it will work out of the box!

### Instantiate our Validator

The code shows how to instantiate a Validator:

```python
from great_expectations.core.batch import BatchRequest

expectation_suite_name = "insert_the_name_of_your_suite_here"

# Setting Batch Request configuration
batch_request_parameters = {
  'datasource_name': 'my_datasource',
  'data_connector_name': 'default_inferred_data_connector_name',
  'data_asset_name': 'my-data-under-test.csv',
  'limit': 1000  # Optional
}

validator = context.get_validator(
    batch_request = BatchRequest(**batch_request_parameters),
    expectation_suite_name = expectation_suite_name
)
```

After we get our Validator instantiated, we can call `validator.head()` to confirm that it contains the data that we expect.

## Checkpoint

In a production deployment of Great Expectations, a Checkpoint serves as the primary means for validating data. Checkpoints provide a convenient abstraction for bundling the Validation of a Batch (or Batches) of data against an Expectation Suite (or several), as well as the Actions (optional) that should be taken after the validation.

Checkpoints have their own Store which is used to persist their configurations to YAML files. These configurations can be committed to version control.

A Checkpoint uses a **Validator** to run one or more Expectation Suites against one or more Batches provided by one or more Batch Requests. Running a Checkpoint produces Validation Results and will result in optional Actions being performed if they are configured to do so.

### Create a Checkpoint

#### Using CLI to open a Jupyter Notebook for creating a new Checkpoint

The Great Expectations CLI has a convenience method that will open a Jupyter Notebook to easily configure and save our Checkpoint. Run the following CLI command from our Data Context:

```bash
great_expectations checkpoint new my_checkpoint
```
We can replace `my_checkpoint` in the above command with whatever name we would like to associate with the Checkpoint we will be creating. After running this command, a Jupyter Notebook will open, which will guide us through the procedure for creating a Checkpoint. We can modify the default setup of this Jupyter Notebook to fit our use case.


### Edit the existing Checkpoint configuration
The following shows the minimum required Checkpoint configuration generated by Jupyter Notebook, which uses the `SimpleCheckpoint class` that takes care of some defaults. The following example shows the YAML configuration as a Python string.

```python
config = """
name: my_checkpoint  # This is populated by the CLI.
config_version: 1
class_name: SimpleCheckpoint
validations:
  - batch_request:
      datasource_name: my_datasource  # Update this value.
      data_connector_name: my_data_connector  # Update this value.
      data_asset_name: MyDataAsset  # Update this value.
      data_connector_query:
        index: -1
    expectation_suite_name: my_suite  # Update this value.
"""
```

We need to replace the names `my_datasource`, `my_data_connector`, `MyDataAsset` and `my_suite` with the respective **Datasource**, **Data Connector**, **Data Asset**, and **Expectation Suite** names we have configured in our `great_expectations.yml`.

### Validate and test our Checkpoint configuration

We can use the following Python statement to validate the contents of our `config` yaml string mentioned above:

```python
context.test_yaml_config(yaml_config=config)
```

Here, `context` represents Data Context object. When executed, `test_yaml_config(...)` will instantiate the passing component and run through a self-check procedure to verify that the component works as expected.

In the case of a Checkpoint, this means:

- Verifying that the Checkpoint class with the given configuration, if valid, can be instantiated.
- Printing warnings in case the configuration is invalid or incomplete.
- Raise error if our configuration was not set up correctly.

### Store our Checkpoint configuration

After we are satisfied with our Checkpoint configuration, we can store it in our Checkpoint Store.

### Run our Checkpoint and open the Data Docs

Before running a Checkpoint, make sure that all classes and Expectation Suites referred to in the configuration exist. We can use the below Python statement to run our Checkpoint.

```python
context.run_checkpoint(...)
```

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

