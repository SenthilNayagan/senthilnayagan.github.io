---
layout: post
title:  "Scala Collections"
kicker: "Scala"
subtitle: "In programming, a collection is an object that groups multiple elements into a single unit. A collection is like a container that can store a variety of items."
image: assets/images/posts-cover-images/scala-collections.jpg
author: senthil
published_on: 2022-08-31 00:00:00 +0530
tags: [ "scala-collections", "scala" ]
categories: scala-collections
featured: false
hidden: true
---

# Introduction to collections

A collection is an object in programming that *groups multiple elements into a single unit*. A collection is comparable to a container that can hold various items. A collection has an underlying data structure that is used to *store*, *manipulate*, and *retrieve* aggregate data in an *efficient manner*. When collections are used, readability and maintenance of code are enhanced.

# Collections in Scala

Scala has a very rich collections library, located under the `scala.collection` package. There are two types of collections in Scala:

- **Mutable collections**
- **Immutable collections**

## Mutable collections

A mutable collection can be *modified* or *extended* in place. This means that we can modify, add, or remove elements from a collection *with side effects*. All mutable collection classes are present in the `scala.collection.mutable` package.

## Immutable collections

In contrast, immutable collections *never change*. There are still operations that simulate additions, deletions, and updates, but each of these operations *returns a new collection and leaves the original collection unchanged*. All immutable collections are present under `scala.collection.immutable`. 