---
layout: post
title:  "Defining Variables Using the `def` Keyword in Scala"
kicker: "Scala"
subtitle: "Difference between `lazy val` and `def`."
image: assets/images/posts-cover-images/scala-def-vs-lazy-val.jpeg
author: senthil
date: 2022-07-21 20:11:10 +0530
tags: [ "programming", "scala", "functional-programming" ]
categories: scala
featured: false
hidden: false
---

The title reads as follows: "*Defining Variables Using the def Keyword.*" Before we continue, let's see if this title is appropriate. It's not!

Let's first be clear that a `def` is not a variable declaration. It's a declaration of a function or method instead. 

Consider the following code:

```shell
scala> def i = 3
def i: Int

scala> i.getClass
val res0: Class[Int] = int

scala> println(i)
3

scala> val j = 3
val j: Int = 3

scala> j.getClass
val res1: Class[Int] = int

scala> println(j)
3

scala> i + j
val res2: Int = 6
```

As we see above, variable definition works using `def`. Unlike a `val` or `var` declaration, a `def` is not a variable declaration. It's a function declaration. What we are doing is creating a function or method that returns a constant number 3. Having said that, both `i` and `j` are functions that do not take any arguments.

Unless the `lazy` keyword is prefixed, variables declared with the `val` or `var` are evaluated immediately, whereas `def` is evaluated on call, which means it is lazy evaluated. Having said, `def` evaluation happens when called explicitly.

> Note that Scala does not permit the creation of lazy variables, i.e., var. Only lazy values (val) are allowed.

# Difference between lazy val and def

|![Lazy Worker.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1658412816599/I9I25eUsl.jpeg?auto=compress,format&format=webp)|
|:-:|
|[<sup>*Designed by Freepik.*</sup>](https://www.freepik.com/free-vector/two-office-workers-looking-sleepy-colleagues-exhausted-employee-sleeping-workplace-flat-vector-illustration-lazy-worker-burnout_10173989.htm)|

Difference between `lazy val` and `def`:
- When accessed for the first time,  `lazy` evaluates and caches the result.
- The `def` declaration is evaluated every time we call method name.

Let's see the difference between lazy val and def in action:

```shell
scala> lazy val a = { println("a value evaluated"); 1}
lazy val a: Int

scala> a
a value evaluated
val res0: Int = 1

scala> a
val res1: Int = 1

scala> def b = { println("b function evaluated"); 2}
def b: Int

scala> a
b function evaluated
val res3: Int = 2

scala> a
b function evaluated
val res4: Int = 2
```

As we see above, `a` is *evaluated only once*, and any subsequent invocations of a return the cached result. This indicates that `lazy val` only needs to be evaluated once and then the result is stored forever.

On the other hand, a `def` is *evaluated every time it is invoked*. In this case, we see `println` output every time we invoke the function.

I hope this explains how `def` differs from `val` in terms of evaluation frequency.

Happy learning!