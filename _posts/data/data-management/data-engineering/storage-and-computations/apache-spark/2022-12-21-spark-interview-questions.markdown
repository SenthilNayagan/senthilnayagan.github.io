---
layout: post
title:  "Apache Spark Interview Questions"
kicker: "APACHE SPARK"
--subtitle: "Apache Pinot is a real-time, distributed OLAP datastore that was built for low-latency, high-throughput analytics, making it perfect for user-facing analytical workloads."
--image: assets/images/posts-cover-images/running-apache-pinot.jpg
author: senthil
date: 2022-12-21 00:00:00 +0530
tags: ["apache-spark", "big-data"]
categories: apache-spark
featured: false
hidden: true
toc: false
---

## What are the different modes available in Apache Spark to handle orrupt records during parsing?

There are *three* different modes available:

- `PERMISSIVE` (Default) - The literal meaning of "permissive" is "allowing" or "not preventing." In this case, all fields are set to `null` and corrupted records are placed in a string column called `_corrupt_record`.
- `DROPMALFORMED` - Drops all rows containing corrupt records. In other words, it ignores or removes invalid data.
- `FAILFAST` - Throws an exception when it encounters corrupted data.

> **Capturing bad data:** To keep corrupt records, an user can set a string type field named `columnNameOfCorruptRecord` (`option("columnNameOfCorruptRecord", "_corrupt_record")`) in an user-defined schema. If a schema does not have the field, it drops corrupt records during parsing. When inferring a schema, it implicitly adds a columnNameOfCorruptRecord field in an output schema.

```python
df = spark.read.option("mode", "PERMISSIVE").csv("/path/to/csv-file.csv", header=True, inferSchema=True)
```

```scala
val df = spark.read.option("header", true).option("mode", "PERMISSIVE").csv("./inputs/employees.csv")
```

## How to bad records and files using  badRecordsPath option?

When we set `badRecordsPath`, all failed records or files that are detected during data loading will be logged to that location. Errors indicating deleted files, network connection exceptions, IO exceptions, and so on are also ignored and logged in the `badRecordsPath`.

### Unable to find input file

```scala
val df = spark.read
  .option("badRecordsPath", "/tmp/badRecordsPath")
  .format("parquet").load("/input/parquetFile")

df.show()
```

- In the above scenario, Spark generates an exception file in JSON format to record the error when `df.show()` unable to locate the input file.
- Here, `bad_files` is the exception type.
- An `xyz` file contains a JSON record, which has the path of the bad file and the exception/reason message.

### Input file contains bad record

```scala
// Creates a json file containing both parsable and corrupted records
Seq("""{"a": 1, "b": 2}""", """{bad-record""").toDF().write.format("text").save("/tmp/input/jsonFile")

val df = spark.read
  .option("badRecordsPath", "/tmp/badRecordsPath")
  .schema("a int, b int")
  .format("json")
  .load("/tmp/input/jsonFile")

df.show()
```

- In this example, the DataFrame contains only the first parsable record (`{"a": 1, "b": 2}`). The second bad record (`{bad-record`) is recorded in the exception file.
- The exception file contains the bad record, the path of the file containing the record, and the exception/reason message.