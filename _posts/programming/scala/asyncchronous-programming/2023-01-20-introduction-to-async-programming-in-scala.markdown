---
layout: post
title:  "Introduction to Asynchronous Programming in Scala"
kicker: "Scala"
subtitle: "Introduction to asynchronouse programming in Scala."
image: assets/images/posts-cover-images/aync-programming.jpg
author: senthil
published_on: 2023-01-20 00:00:00 +0530
tags: ["scala", "asynchronous", "async", "asynchronouse-programming", "scala-async"]
categories: scala
featured: false
hidden: true
toc: true
draft: true
---

# Overview

Because it might be difficult to understand at first, "asynchronous" is a topic that both beginner and seasoned developers frequently pay less attention to or totally avoid. Another reality is that we frequently work toward achieving the desired program results without paying as much attention to identifying the most effective means of achieving those results. Naturally, when we think of the intended result, the lines of instructions or statements are the first that come to mind. We begin with a statement-focused approach as opposed to a performance-focused one.

Let's first understand certain fundamentals before moving on to the concept of asynchronous programming:

- **What is a thread in programming?**
- **What is multi-thread?**
- **How a thread differs from a process?**

## What is a thread in programming?

An application can construct multiple execution paths very easily by using threads.  A thread itself is not a program or application. This means, unlike a program, a thread cannot run on its own. A thread is the smallest and most lightweight sequence of instructions intended to be scheduled and executed by an operating system component called a *scheduler* independently of the parent process. Simply put, a thread is a sequential flow of tasks within a process. We can think of a process as a container where more than one thread runs.

> **Program vs. process:** I often use the terms "program" and "process" interchangeably. Please be aware that these terms are not the same in practice.<br/><br/>A program is a passive (not active) entity that contains the set of instructions that are used to carry out particular tasks. These instructions are stored in a file or files in secondary memory, such as a disk.<br/><br/>When a program is executed, an active instance of the program is launched in the primary memory (RAM). This active instance is referred to as a process.

Every program has one or more threads of execution, which can be used to carry out various tasks concurrently or almost concurrently. As mentioned above, usually the operating system itself manages these threads of execution, scheduling them to run on the available cores and *preemptively* interrupting (stoping) them as necessary. Are we saying "stop the running threads"? Yes, you got it right. The need for interruption will be discussed further down, but first, let's understand what *preemption* in threading means.

> **Preemption:** Preemption in computing is the act of temporarily interrupting (stoping) an executing task with the intention of resuming it later from where it left off. Typically, this interruption is done by an external scheduler with no assistance or cooperation from the task. Here comes a question: *Why should a running thread be preempted?* The running thread is often preempted when a high-priority thread is added to the *ready queue*. A thread typically enters the READY state after its block condition is resolved. If two threads of the same priority are waiting for the CPU, the scheduler randomly chooses one of them to run. We will have no control over which goes first. So the objective of preempting a running thread is to free up resources so that other threads can execute.

### Components of thread

A thread has the following three components:

- Set of **registers**
- A **program counter** (PC)
- A **stack space**

#### Reister

Registers are a type of high-speed storing memory that are part of the computer processor (CPU). An instruction, a storage address, or any other type of data could be stored in a register. A register must be large enough to hold an instruction; for instance, on a 64-bit computer, a register must be 64 bits long.

A processor often contains several kinds of registers, which can be classified based on the values they can store or the instructions that can be executed on them. Some of the most common registers used are as follows:

- **Program counter**
- **Accumulator**
- **Address register**
- **Data register**
- **Temporary register**
- **Input register**
- **Output register**

### Motivation

- **Multi tasks:** In modern programming, threads are immensely useful anytime a process needs to perform multiple tasks independently of one another.
- **Non-blocking nature:** Threads offer a non-blocking approach, allowing the other tasks to continue without interruption when one of the tasks blocks.