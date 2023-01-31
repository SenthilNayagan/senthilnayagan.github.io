---
layout: post
title:  "Introduction to Asynchronous Programming in Scala"
kicker: "Scala"
subtitle: "Introduction to asynchronouse programming in Scala."
image: assets/images/posts-cover-images/aync-programming.jpg
author: senthil
--author_contacts: false
published_on: 2023-01-20 00:00:00 +0530
tags: ["scala", "asynchronous", "async", "asynchronouse-programming", "scala-async"]
categories: scala
featured: false
hidden: true
toc: true
draft: true
---

# Overview

Because it might be difficult to understand at first, "asynchronous" is a topic that both beginners and seasoned developers frequently pay less attention to or totally avoid. Another reality is that we frequently work toward achieving the desired program results without paying as much attention to identifying the most effective means of achieving those results. Naturally, when we think of the intended result, the lines of instructions or statements are the first that come to mind. We begin with a statement-focused approach as opposed to a performance-focused one.

Let's first understand certain fundamentals before moving on to the concept of asynchronous programming:

- **A program vs. a process**
- **What is a thread in programming?**
- **What is multi-thread?**
- **How a thread differs from a process?**

## Program vs process

Although I may often use the terms "program" and "process" interchangeably here, they are not the same in practice. **A program is  passive (not active) in nature** because it does nothing until it is executed. It contains the set of instructions that are used to carry out particular tasks. These instructions are stored in a file or files in secondary memory, such as on a disk. A program *starts*, *executes a series of instructions*, and *ends*. When a program is executed, an active instance of the program is launched in the primary memory (RAM). This active instance is referred to as a process. So **a process is a programme in execution**. Unlike a program, a process needs resources like a CPU, memory, and IO to perform the task.

## What is a thread in programming?

A program or an application *starts*, *executes a series of instructions*, and *ends*. This is referred to as a program's life cycle. With a single-core processor, the instructions are carried out in a traditional monolithic manner: sequentially, one after the other. As multi-core computers become more common these days, a programme can construct *multiple execution paths* very easily by using *threads*.

|![Figure 1: An application's life cycle](/assets/images/posts/a-program-starts-executes-ends.png){: width="80%" }{: style="float: center" }|
|:-:|
|<sup>*Figure 1: An application's life cycle.*</sup>|<br/><br/>

A thread itself is not a program or application. This means, unlike a program, **a thread cannot run on its own**. A thread is the smallest and most lightweight *sequence of instructions* intended to be scheduled and executed by an operating system component called a *scheduler* independently of the parent process. Simply put, **a thread is a sequential flow of tasks within a process**. We can think of a process as a container where one or more threads run. 

**It's like french fries in a box, where the fries are threads and the box is the process** :-)

|![Figure 2: French fries in a box](/assets/images/posts/fries-in-a-box.png){: width="40%" }{: style="float: center" }|
|:-:|
|<sup>*Figure 2: French fries in a box.*</sup>|<br/><br/>

From a thread standpoint, there is a possibility of only one french fry in a box, though we don't like it and definitely want more. It's referred to as a **single-threaded process**. Having said that, **every process has at least one thread**. 

Having said that, a program under execution is referred to as a process, and **a threat is a fundamental unit of execution**. Since a thread deals solely with the execution of tasks, we can also say that **a thread is a fundamental unit of CPU utilisation**. Keep in mind that each CPU only executes one thread at a time.

> **Definition:** A thread is a fundamental unit of CPU execution. It's a single sequential flow of control within a process.

### Processes vs. threads

We are aware that a process and a thread are not the same now. Processes and threads vary from one another in the following ways:

- A process is typically a big, independent entity, while threads exist as an integral part of a process.
- A process carries a lot more state information than threads do, and multiple threads inside a process share the process state. In other words, threads share the process's resources, including memory and open files.
- Context switching most likely takes longer for processes while it takes less time for threads.
- Compared to threads, processes use more resources.
- The creation and termination of a process takes longer than that of a thread.

|![Figure 3: A process with two threads of execution](/assets/images/posts/process-and-threads.png){: width="50%" }|
|:-:|
|<sup>*Figure 3: A process with two threads of execution.*</sup>|<br/><br/>

From the above thread definition, let's understand what "flow of control" is.

> **Flow of control:** In computer programming, "control flow" or "flow of control" refers to the order in which a program's statements are executed or evaluated when a program is running.

Every program has one or more *threads of execution*, which can be used to carry out various tasks *concurrently* or *almost concurrently*. As mentioned above, usually the operating system itself manages these threads of execution, scheduling them to run on the available cores and *preemptively* interrupting (stoping) them as necessary. Are we saying "stop the running threads"? Yes, you got it right. The need for interruption will be discussed further down, but first, let's understand what *preemption* in threading means.

> **Preemption:** Preemption in computing is the act of temporarily interrupting (stoping) an executing task with the intention of resuming it later from where it left off. 

Typically, this interruption is done by an external scheduler with no assistance or cooperation from the task. Here comes a question: **Why should a running thread be preempted?** The running thread is often preempted when a high-priority thread is added to the *ready queue*. A thread typically enters the READY state after its block condition is resolved. If two threads of the same priority are waiting for the CPU, the scheduler randomly chooses one of them to run. We will have no control over which goes first. So the objective of preempting a running thread is to free up resources so that other threads can execute. **TODO:** More more about context switching for concurrency.

Before continuing, it's crucial to understand how concurrency and parallelism differ from one another.

### Concurrency and parallelism

TODO


### Components of thread

A thread must allocate some of its own resources inside a running program. For instance, it must have its own *execution stack* and *program counter*. Having said that, a thread comprises the following components:

- A **thread ID**
- Set of **registers**, including a **program counter** (PC)
- A **stack space**

Apart from these, a thread shares with other threads belonging to the same process: 

- its **code**
- its **data**
- **other operating system resources**, such as open files and signals

Let's understand each of the main components in a little more detail.

#### Registers

Registers are a type of high-speed, small amout of memory storage *contained within the computer processor (CPU)*. It's a component of the CPU and not RAM, or main memory.

|![Figure 4: Registers are part of CPU](/assets/images/posts/cpu-components.png){: width="50%" }{: style="float: center" }|
|:-:|
|<sup>*Figure 4: Registers are integral part of CPU.*</sup>|<br/><br/>

The processor makes use of these registers to store small amounts of data that are needed during processing, such as:

- Current instruction being decoded
- Address of the next instruction to be executed
- Results of calculations

A register must be large enough to hold an instruction; for instance, on a 64-bit computer, a register must be 64 bits long. A processor often contains several kinds of registers, which can be classified based on the values they can store or the instructions that can be executed on them. The number of registers used by different processors varies, although the majority of them have some—if not all—of the following:

- **Program counter**
- **Accumulator**
- **Address register**
- **Data register**
- **Temporary register**
- **Input register**
- **Output register**

### Types of thread

TODO

### Single thread vs. multi thread

A single-threaded process (a process with only one thread of execution) follows a single sequence of control while executing, but a multi-threaded process has several sequences of control and is thus able to perform multiple independent tasks concurrently. The aim of multithreading is to split a single process into multiple threads as opposed to starting a whole new process. Multithreading is used to achieve real parallelism and improve the performance of the application.

|![Figure 5: Single-threaded process vs. multi-threaded process](/assets/images/posts/single-threaded-vs-multi-threaded-processes.png){: width="90%" }|
|:-:|
|<sup>*Figure 5: Single-threaded process vs. multi-threaded process.*</sup>|<br/><br/>

Each thread in a multi-threaded process has its own **program counter**, **stack**, and **set of registers**, but they all share common **code**, **data**, and **OS resources** like open files.

> **Process-based multitasking**: There are two distinct types of multitasking: process-based and thread-based. The distinction between the two must be understood. As mentioned earlier, a process is essentially an active program (under execution). A process-based multitasking is a capability that enables our computer to run two or more programs concurrently. For instance, using an IDE or text editor while running any compiler is possible with process-based multitasking. In process-based multitasking, an entire program itself is the smallest unit of code that can be managed by the OS scheduler. While thread-based multitasking takes care of the details, process-based multitasking deals with the big picture.

### Motivation

- **Multi tasks:** In modern programming, threads are immensely useful anytime a process needs to perform multiple tasks independently of one another.
- **Non-blocking nature:** Threads offer a non-blocking approach, allowing the other tasks to continue without interruption when one of the tasks blocks.

# References

- https://medium.com/swift-india/concurrency-parallelism-threads-processes-async-and-sync-related-39fd951bc61d
- https://jenkov.com/tutorials/java-concurrency/single-threaded-concurrency.html#:~:text=Single%2Dthreaded%20Concurrency%20means%20making,progress%20on%20its%20own%20task
- https://jenkov.com/tutorials/java-concurrency/concurrency-vs-parallelism.html
- https://www.ibm.com/docs/en/aix/7.2?topic=programming-understanding-threads-processes
- https://pages.mtu.edu/~shene/NSF-3/e-Book/FUNDAMENTALS/thread-management.html
