---
layout: post
title:  "How To Set SLA in Apache Airflow"
kicker: "DevOps"
subtitle: "Apache Airflow enables us to schedule tasks as code. In Airflow, a SLA determines the maximum completion time for a task or DAG. Note that SLAs are established based on the DAG execution date, not the task start time."
image: assets/images/posts-cover-images/airflow-setting-sla.jpg
featured_image: assets/images/featured-images/airflow-setting-sla.jpg
imageshadow: true
author: senthil
date: 2022-07-24 20:00:01 +0530
tags: ["airflow", "sla", "service-level-agreement", "workflow-engine"]
categories: orchestration
featured: true
hidden: false
---

# A quick overview of Airflow

Apache Airflow is, at its core, an open-source *batch* task scheduler. It's a platform for *programmatically* *authoring* (creating), *scheduling*, and *monitoring* workflows via Directed Acyclic Graphs (DAGs). Airflow is distinguished from other schedulers primarily by its ability to schedule *tasks as code*. It's gaining popularity, especially among developers, because it's written in Python and it's easy to create code-based orchestration with it. To put it all together, Airflow is a batch-oriented open-source framework that enables us to build data pipelines and monitor data workflows efficiently.

# How to set SLA in Airflow

SLA stands for **service-level agreement**. It's a *contract* between a service provider and its customer that outlines *the level of service expected by the customer and guaranteed by the vendor*. 

In the context of Airflow, an SLA is the maximum amount of time a task or a DAG should take to complete. It serves to notify us if our DAG or tasks are taking longer than expected to complete.

> **Note:** SLAs are set to the DAG execution date rather than the task start time.

# SLA misses

An SLA miss occurs if a task or DAG is not finished within the allotted time for the SLA. 

When SLAs are breached, Airflow can perform the following:

- **Sends an email alert notification**
- **Logs the event in the database** - the event is also logged in the database and made available in the web UI. The web UI gives us broad details about which task failed to meet the SLA and when. Additionally, it shows if an email was sent after the SLA failed.
- **Invokes the callback function** via `sla_miss_callback`.

> **Note:** It should be noted that only scheduled tasks will be checked against the SLA. An SLA miss won't be triggered by manually initiated tasks!

# Defining SLAs

There are several ways to define an SLA that include:

- Defining SLA on the task itself
- Defining SLA on DAG level

## Defining SLA on the task itself

Note that defining SLAs for a task is optional. As a result, in a DAG with several tasks, some tasks may have SLAs and others may not. To set an SLA for a task, pass a `datetime.timedelta` object to the task/operator’s `sla` parameter as shown below:

{% highlight python %}
from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from datetime import datetime, timedelta

with DAG('setting_sla', 
        start_date=datetime(2002, 1, 1), 
        schedule_interval='@daily',
        catchup=False) as dag:
        
    bash_task = BashOperator(
        task_id='bash_task',
        bash_command='sleep 10',
        sla=timedelta(seconds=5)  # SLA for the task
    )
{% endhighlight %}

> When a task exceeds its SLA, it is not cancelled. It's free to continue running till the end.

The `timedelta` object is found in the Python's datetime library. It takes the following arguments:

- microseconds (not applicable to Airflow)
- milliseconds (not applicable to Airflow)
- seconds
- minutes
- hours
- days
- weeks

Note that milliseconds and microseconds available, but those wouldn't apply to Airflow.

### Cancelling or failing the task

Timeouts can help cancel or fail a task if it takes longer than the allotted time. If we want to cancel a task after a certain runtime is reached, we can set the `execution_timeout` attribute to a `datetime.timedelta` value that represents the longest permitted duration.

Having said that, the maximum duration permitted for each execution is managed by the property `execution_timeout`. If it's breached, the task times out and `AirflowTaskTimeout` is raised.

# Implementing our own logic

We can use callbacks to trigger our own logic if we choose to do so. In this case, we can use `sla_miss_callback` that will be called when the SLA is not met.

> Only one callback function is permitted for tasks or a DAG.

The function signature of an `sla_miss_callback` requires the following five parameters:
- `dag`
- `task_list`
- `blocking_task_list`
- `slas`
- `blocking_tis`

Let's define a callback method:
```python
def _sla_missied_take_action(*args, **kwargs):
    logger.info(args)
```

Pass the callback method to DAG as shown below:
```python
with DAG('setting_sla', 
    start_date=datetime(2002, 1, 1),
    default_args=default_args,
    schedule_interval='@daily',
    sla_miss_callback=_sla_missied_take_action  # callback function
    catchup=False) as dag:
```

# Disabling SLA

If we want to disable SLA checking entirely, we can set `check_slas = False` in Airflow’s `[core]` configuration.
```text
[core]
check_slas = False
```