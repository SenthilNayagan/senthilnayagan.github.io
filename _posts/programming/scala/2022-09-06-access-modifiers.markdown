---
layout: post
title:  "Access Modifiers in Scala"
kicker: "Scala"
subtitle: "Access modifiers, also known as access specifiers, determine the accessibility and scope of classes, methods, and other members. Scala's access modifiers closely resemble those of Java, although they provide more granular and powerful visibility control than Java."
image: assets/images/posts-cover-images/access-modifiers-in-scala.jpg
author: senthil
date: 2022-09-06 00:00:00 +0530
tags: [ "access-modifiers", "scala", "oops" ]
categories: scala
featured: false
hidden: false
toc: true
---

# What is access modifiers in OOP?

Before we proceed, let's know what access modifiers are in object-oriented programming.

Access modifiers, also known as *access specifiers*, determine the **accessibility** and **scope** of *classes*, *methods*, and *other members*. The encapsulation principle of object-oriented programming is managed through the use of access modifiers.

In general, access modifiers are stated using the following keywords:

- **Public**
- **Private**
- **Protected**
- **Package**

Note that if we try to refer to an *inaccessible* member, we'll usually get a compile-time error!

# Access modifiers in Scala

Scala's access modifiers closely resemble those of Java, although they provide more *granular* and *powerful* visibility control than Java.

## Public

There is *no explicit* modifier for *public members* in Scala. Any member that is not labeled as *private* or *protected* is public, making all members *public by default*. Public members are accessible from any anywhere.

## Private

Private Scala members are treated similarly to private Java members. A *private* member is only accessible to the current class or instance and also other instances of the same class in which it is specified. This implies that private members of a parent class are *not available* to sub-classes (aka derived classes).

```scala
class MyClass {
  private var myFlag: Boolean = false
}

class MySubClass extends MyClass {
  def myMethod(): Unit = {
    // Note that private members are not available to sub-classes.
    // Hence, the below statement won't compile as it is trying to
    // access the private member, myFlag of the base class.

    println(myFlag) // won't compile!
  }
}
```

## Object-private

Scala's object-private *goes beyond private scope* to make fields and methods object-private, extending the level of privacy. **It's the most restrictive access**. 

Mark a method as object-private by placing the access modifier `private[this]` before the method declaration:

```scala
private[this] def isMaxVal = true
```

This makes the method accessible **only** from within the same object that contains the definition; other instances of the same class cannot access the method.

Note that the other instances of the same class cannot access them:

```scala
class Cat {
  var breed: String = "Persian"
  private[this] var sex: String = "Male"

  def showDetails(other: Cat): Unit = {
    println(other.breed)

    // Below statement won't compile as we are accessing
    // via other instance of the same class.
    //println(other.sex)
  }
}
```

## Protected

Accessing protected members in Scala is a bit more restrictive than in Java. In Java, protected members can be accessed by other classes in the same package, but this is *not true* in Scala.

In Scala, protected members can be accessible from:

- Within the class.
- Within its subclasses.
- Within the companion objects.

```scala
class MyBaseClass {
  protected var myFlag: Boolean = false

  def showFlag(): Unit = {
    println("myFlag: " + myFlag) // Accessible from within the class
  }
}

class MySubClass extends MyBaseClass {
  def showDetails(): Unit = {
    // Unlike private, we can access protected member from subclass.
    println(myFlag)
  }
}
```

The following code won't compile even though both the classes are in the same package:

```scala
package planet {
    class Bird {
        protected def fly {}
    }
    class Forest {
        val bird = new Bird
        bird.fly   // error: this line won't compile
    }
}
```

The above code won’t compile because the `Forest` class can’t access the `fly` method of the `Bird` class, even though they’re in the same package.

## Package or private[package]

To make a method available to all members of the current package, mark the method as being private to the current package by specifying it using the `private[packageName]` syntax:

```scala
package org.earlycode.scalatutorial.basics {
  class Fruit {
    private[basics] def doChop {}
    private def doEat {}
  }

  class PackageScope {
    val fruitObj = new Fruit

    // Access by other classes in the same package 
    // i.e. basics package.
    fruitObj.doChop

    // Below statement won't compile as doEat method is available
    // only to the Fruit class
    //fruitObj.doEat
  }
}
```

## Quick reference

|![Quick Reference of Access Specifiers](/assets/images/posts/scala-quick-reference-access-modifiers.png "Quick Reference of Access Specifiers")|
|:-:|
|<sup>*Figure 1: Quick Reference of Access Specifiers.*</sup>|<br/><br/>

## Default access modifiers

Scala’s default access modifier is public. As metioned earlier, there is no explicit modifier for public members; any member not labelled private or protected is public. Public members can be accessed from anywhere.

# Access modifiers - differences and similarities with Java

## Differences

- **Public** — Unlike Java, Scala has no explicit modifier for public members.
- **Protected members** in Java have wider access than in Scala; Java’s protected members can be accessible not only from within the class or within its subclasses, but also from other classes in the same package.
- **Default access modifier** — Java defaults to package internal visibility, while Scala, on the other hand, defaults to public, i.e. can be accessed from anywhere.
- Java adopts an all-or-nothing access strategy, i.e., either it’s visible to all classes in the current package or it’s not visible to any, whereas Scala gives fine-grained control over visibility.
- **Object-private scope** — Scala `private[this]` takes privacy a step further than private scope and makes the fields and methods object-private which means they can only be accessed from the object that contains them.

## Similarities

- Private members in Scala are treated similarly to Java.