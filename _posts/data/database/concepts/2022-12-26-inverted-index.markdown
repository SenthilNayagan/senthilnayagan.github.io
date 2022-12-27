---
layout: post
title:  "Inverted Index"
kicker: "Database"
subtitle: "???."
image: assets/images/posts-cover-images/inverted-index.jpg
author: senthil
date: 2022-12-26 00:00:00 +0530
tags: [ "inverted-index", "index" ]
categories: Database
featured: false
hidden: true
toc: false
---

# What is an inverted index?

All modern **information retrieval systems** (**IRS**) rely heavily on inverted indexes, a kind of data structure that is essential to effective retrieval. It stores a mapping of words (or any search terms) to their locations in the database table or document. In other words, it maintains a key-value pair that identifies where in the database or document a given search phrase may be found. The purpose of an inverted index is to allow fast full-text searches, but at the expense of additional processing time whenever a new item is added to the database.

| Term   || Document/Table ID |
|--------||-------------------|
| large  || 1001, 1003        |
| winter || 1002              |
| apple  || 1002, 1004        |
| hot    || 1002, 1003, 1005  |

<sup>*Table 1: Search Terms Mapped to Document/Table IDs.*</sup>


Now comes the obvious question: **Which data structure is best?** Can we use a fixed-size data structure such as an array for this? Well, fixed-size data structures, such as fixed-sized arrays, are impractical.  If we consider a *dynamic index* in which new documents are added or existing ones are modified, it will be challenging to modify the sizes of the fixed-data structures. For these reasons, we need a linked list or equivalent data structure with a dynamically changing size. In standard information retrieval terminology, this list is called a **posting list** (also referred to as a **postings file**, or **inverted file**). 

In memory, a posting list can be represented as a data structure such as a linked list or an array with variable length. One occurrence of a word-document pair (large => 1001, 1003) is referred to as a **posting**, and the sum of all the posting lists is then referred to as the **postings**.
