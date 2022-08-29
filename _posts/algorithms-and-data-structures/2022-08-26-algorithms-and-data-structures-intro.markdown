---
layout: post
title:  "An Introduction to Algorithms and Data Structures"
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

## Big-O notation

Big O notation uses algebraic terms to describe the *complexity of our code*.

No mathematical knowledge is necessary to understand Big-O notation. Each Big-O notation defines a curve's (XY graph) shape. The shape of the curve represents the relationship between the *size* of a dataset (amount of data) and the *time* it takes to process that data.

The performance of the following Big-O notations is ranked from best (`O(1)`) to worst (`O(n!)`):

- **O(1)**
- **O(log(n))**
- **O(n)**
- **O(n log(n))**
- **O(n^2)**
- **O(2^n)**
- **O(n!)**

## Algorithm efficiency

A problem can be solved in more than one way. So, it is possible to come up with more than one solution (algorithm) for a given problem. We must compare them to find the efficient one.

There are two measures of efficiency when it comes to algorithms:

- **Time**
- **Space**

### Time complexity (Speed)

The efficiency measured in terms of time is known as time complexity. Time complexity is a measure of how long an algorithm takes to execute. Time complexity is defined as a *function of the input size `n` using Big-O notation as `O(n)`*, where `n` is the size of the input and `O` is the growth rate function for the worst-case scenario.

> **Big-O notation:** The Big-O notation enables a standard comparison of the worst-case performance (aka upper bound) of our algorithms.

#### Constant time complexity

If an algorithm's time complexity is *constant*, its execution time remains the same regardless of the *input size* and the *number of times it is executed*. The constant time is denoted by `O(1)` in Big-O notation. If we can create an algorithm that solves the problem in `O(1)`, we are likely at our top performance. Note that `O(1)` has the least complexity.


### Space complexity (Memory)

Space complexity is primarily a computer area of concern. It corresponds to the amount of memory a computer uses.

An effective algorithm must strike a balance between these two measures. For example, it might not matter if we have a super-fast algorithm if it uses up all the memory we have.

> **Ideal efficiency:** The most efficient algorithm could be one that performs all of its operations in the shortest time possible and uses the least amount of memory.

## Linear search algorithm

For a given value of `n`, linear search will require `n` attempts to locate the value in the worst case. By evaluating a worst-case scenario, we avoid having to perform the necessary work. We are aware of the actual or potential outcome. As the value gets really large, the running time of the algorithm gets large as well.

# What is data structure?

A data structure is a way to *organize*, *manage*, and *store data* that makes it *easy* to access its values and change them. To efficiently access or utilize data, we must first store it efficiently.

Data structures are essential ingredients in creating fast and powerful algorithms. They make code cleaner and easier to understand.

## Types of data structure

Generally, data structures fall into two categories:

- **Linear data structure**
- **Non-linear data structure**

### Linear data structure

Data elements are arranged *sequentially* in a linear data structure. These elements can be *ordered in any manner* (ascending or descending). In a linear data structure, each element must have an element before it, and if it is not the last element, it may also have an element after it. 

### Non-linear data structure

TODO

## Asymptotic analysis

## FAQ

### Array vs. linked list

Inserting an element at the beginning of the sequential list is way faster in the *linked list* than in the *array*.

### O(1) vs. O(log(n))

`O(log(n))` is more complex than `O(1)`. `O(1)` indicates that the execution time of an algorithm is *independent* of the size of the input, whereas `O(log n)` denotes that as input size `n` increases exponentially, the running time increases linearly.

In some instances, `O(log(n))` may be faster than `O(1)`, but `O(1)` will outperform `O(log(n))` as `n` grows, because `O(1)` is independent of the input size `n`.