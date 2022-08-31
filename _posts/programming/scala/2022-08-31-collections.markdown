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

> **Scala collections vs Java collections:** Scala collections are notably distinct from Java collections. For example, the Scala `List` class differs significantly from the Java `List` classes, including the immutable nature of the Scala `List`.

## Mutable collections

A mutable collection can be *modified* or *extended* in place. This means that we can modify, add, or remove elements from a collection *with side effects*. All mutable collection classes are present in the `scala.collection.mutable` package.

## Immutable collections

In contrast, immutable collections *never change*. There are still operations that simulate additions, deletions, and updates, but each of these operations *returns a new collection and leaves the original collection unchanged*. All immutable collections are present under `scala.collection.immutable`. 

The collections in `scala.collection` are supertypes of `scala.collection.mutable` and `scala.collection.immutable`. The base operations are added to the types in the `scala.collection` package, while the immutable and mutable operations are added to the types in the other two packages.

## A high-level view of the Scala collections

TODO

## Collections table

![](/assets/images/posts/scala-collections-characteristics-table.png)

## Frequently asked questions (FAQ)

### What is `InPlace` transformation operation?

Instead of *always returning a new collection* after a `map` or `filter` operation, mutable collections have a couple of new operations (`filterInPlace` and `mapInPlace`) that let us *change the elements right where they are* (in-place). These *new operations change the source collection*.

```scala
val buf1 = ArrayBuffer(0, 1, 2, 3, 5, 6, 8, 9)
buf1.filterInPlace(_ % 2 == 0) 
buf1 // ArrayBuffer(0, 2, 6, 8)

val buf2 = ArrayBuffer(0, 1, 2, 3, 5, 6, 8, 9)
buf2.mapInPlace(_ * 2)
buf2 // ArrayBuffer(0, 2, 4, 6, 10, 12, 16, 18)
```

### What are *trasnformer* methods?

Transformer methods are collection methods that transform an input collection into a new output collection based on an algorithm we provide.

The transformer methods are as follows:

- **`map`**
- **`filter`**
- **`reverse`**

