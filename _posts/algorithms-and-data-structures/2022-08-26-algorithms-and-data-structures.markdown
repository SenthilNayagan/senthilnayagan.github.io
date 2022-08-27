---
layout: post
title:  "Algorithms and Data Structures"
kicker: "Algorithms and Data Structures"
subtitle: "An algorithm is a series of instructions in a particular order for performing a specific task."
image: assets/images/posts-cover-images/algorithms-and-data-structures.jpg
author: senthil
published_on: 2022-08-26 00:00:00 +0530
tags: ["algorithms-and-data-structures", "algorithms", "data-structures"]
categories: algorithms-and-data-structures
featured: false
hidden: true
---

# What is an algorithm?

An algorithm is a set of steps or instructions in a particular order for performing a specific task. 

If we have ever written code before, we have written an algorithm. The code can be considered an algorithm. There are multiple problems in computer science, but some of them are pretty common regardless of the project we're working on. Different people have developed various solutions to these common problems, and the field of computer science has, over time, identified several that perform well for a particular task.

## What characteristics or guidelines do algorithms possess?

- An algorithm should have a clearly defined problem statement, input, and output.
- The algorithm's steps must be performed in a very specific order.
- The steps of an algorithm must be distinct; it should not be possible to subdivide them further.
- The  algorithm should produce a result; each instance of a given input must produce the same output *every time*.
- The algorithm must finish within a finite amount of time.

These guidelines not only aid in defining what an algorithm is, but also in validating its correctness.

## Algorithm efficiency

There are two measures of efficiency when it comes to algorithms:

- **Time**
- **Space**

### Time complexity

The efficiency measured in terms of time is known as time complexity. Time complexity is a measure of how long an algorithm takes to execute.

### Space complexity

Space complexity is primarily a computer area of concern. It corresponds to the amount of memory a computer uses.

An effective algorithm must strike a balance between these two measures. For example, it might not matter if we have a super-fast algorithm if it uses up all the memory we have.

## Linear search algorithm

For a given value of `n`, linear search will require `n` attempts to locate the value in the worst case. By evaluating a worst-case scenario, we avoid having to perform the necessary work. We are aware of the actual or potential outcome. As the value gets really large, the running time of the algorithm gets large as well.

# What is data structure?

A data structure is a way to *manage*, *organize*, and *store data* that makes it *easy* to access its values and change them.

## Types of data structure

Generally, data structures fall into two categories:

- **Linear data structure**
- **Non-linear data structure**

### Linear data structure

Elements are arranged sequentially in linear data structures. These elements can be ordered by any sort. Each element in a linear data structure must have an element preceding it and may have an element following it if it is not the final element.