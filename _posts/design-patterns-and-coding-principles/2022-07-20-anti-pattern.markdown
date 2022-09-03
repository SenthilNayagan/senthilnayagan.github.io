---
layout: post
title:  "Anti-Pattern"
kicker: "Design Patterns and Coding Principles"
subtitle: "Anti-patterns at first seem to be quick and reasonable, they typically have adverse effects in the future. They are design and code smells. It affects our software badly and adds technical debt. We should avoid them at all costs."
image: assets/images/posts-cover-images/antipattern.webp
author: senthil
date: 2022-07-20 11:24:20 +0530
last_modified_on: 2022-07-30 00:00:00 +0530
tags: ["anti-pattern", "design-patterns"]
categories: design-patterns-and-coding-principles
featured: false
hidden: false
rating: false
---

## What is an anti-pattern?

The term "anti-pattern" was first used in 1995 by a computer programmer, Andrew Koenig, in an article called "Journal of Object-Oriented Programming."

> An antipattern is just like a pattern, except that instead of a solution it gives something that looks superficially like a solution but isn't one. - **Andrew Koenig**

Anti-patterns in software engineering are a commonly used, simple-to-implement solution to recurring issues that is often inefficient and has the potential of being incredibly counterproductive. It demonstrates how to go from a problem to a bad solution. We just call these *bad ideas* or *design smells*. Avoid them at all costs!

### Anti-patterns are undesirable counterparts to design-patterns

Anti-patterns are the undesirable opposites of *design patterns*, which are formalized solutions to frequent issues and are usually viewed as acceptable development practice. Although anti-patterns at first seem to be quick and reasonable, they typically have adverse effects in the future. We will eventually come to understand that anti-pattern results in more negative outcomes than positive ones.

### Anti-patterns might result in technical debt

It affects our software badly and adds *technical debt*! Technical debt is a development decision taken for short-term gains that has long-term consequences. If we let our unintentional technical debt to grow wildly, it may result in dissatisfied developers and encourage staff attrition. IT leaders see technical debt as a significant threat to their organizations' capabilities. 

### What do anti-patterns reveal to us?

A well formulated anti-pattern reveals us:

- Why does the poor solution seem appealing?
- Why it ends up being bad?
- What best patterns are available to replace it?

> The issue that the anti-pattern is attempting to solve already has a better alternative solution in place that is proven to be effective in contrast to the anti-pattern.

### In one aspect, the bad is perceived as the good

In general, developers are not too concerned about identifying design and code smells. However, it's important to keep in mind that identifying bad practices can be as valuable as identifying good practices.

### Examples of anti-patterns

Here are some examples of anti-patterns:

- **Spaghetti code** - It's an unstructured and difficult-to-maintain source code.
- **Golden hammer** - It is the practice of applying a known strategy to a variety of issues.
- **God object/class** - This class or object is responsible for too many things.
- **Boat anchor** - It is future code that has no use inside the present context.
- **Magic numbers and strings** - Using unnamed numbers or string literals instead of named constants in code.


Lastly, it should be noted that the same solution may act as both a pattern and an anti-pattern, depending on the context.