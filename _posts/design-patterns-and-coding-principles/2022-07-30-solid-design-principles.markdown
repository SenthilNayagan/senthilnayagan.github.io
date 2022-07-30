---
layout: post
title:  "SOLID Design Principles"
kicker: "Design Patterns and Coding Principles"
subtitle: "SOLID is a mnemonic acronym that refers to a set of design principles developed for object-oriented programming languages. It makes our code more readable, extensible, and manageable when correctly implemented."
image: assets/images/posts-cover-images/solid-design-principles.jpg
author: senthil
published_on: 2022-07-30 00:00:00 +0530
tags: ["design-patterns", "singleton-pattern", "coding-principles", "solid"]
categories: design-patterns solid
permalink: /:categories/:title
featured: false
hidden: false
---

The term "SOLID" in software engineering refers to a group of five design principles that are meant to improve the *readability*, *flexibility*, and *maintainability* of object-oriented designs.

The SOLID design principle includes the following five principles:
- **S**ingle-responsibility principle
- **O**pen-closed principle
- **L**iskov substitution principle
- **I**nterface segregation principle
- **D**ependency inversion principle

American software engineer and instructor Robert C. Martin (aka Uncle Bob) explains and promotes these design principles. We will examine each of them to learn how these principles can aid in the creation of software that is well-designed.

# Single-Responsibility Principle (SRP)
Robert C. Martin, the originator of this principle, put it this way: "*A class should have only one reason to change.*" It means that every module, class, or function should only have one (single) responsibility.

|![Single-Reponsibility Principle](/assets/images/posts/solid-srp.jpg)|
|:-:|
|<sub><sup>*SOLID - Single-Reponsibility Principle.*</sup></sub>|


Martin defines *responsibility* as a *reason to change* and concludes that a class or module should have a single and unique reason to change.

A class with multiple responsibilities has a higher chance of having bugs since altering one of those responsibilities might have unintended effects (side-effects) on the others. Utilising this principle makes code easier to test and maintain.

## Motivation
- 