---
layout: post
title:  "Singleton Pattern"
kicker: "Design Patterns and Coding Principles"
subtitle: "A singleton pattern limits the number of instances of a class to one."
image: assets/images/posts-cover-images/singleton-pattern.jpg
author: senthil
date: 2022-07-21 22:05:40 +0530
tags: ["design-patterns", "singleton-pattern", "coding-principles"]
categories: design-patterns-and-coding-principles
featured: false
hidden: false
---

A singleton pattern limits the number of instances of a class to one. It falls under a *creational design pattern* because it deals with an object creation procedure. The singleton pattern ensures that a class has only one instance and provides a *global access point* to it.

> **Design patterns:** In software engineering, a *design pattern* is a general, reusable solution to a commonly occurring problem in software design. It's not a fully finished design that can be put into source code right away. Instead, it serves as a description, model, or template for problem-solving that may be used in a variety of situations.

Except for the special creation method (magic method in Python), the singleton pattern prevents the creation of objects via any other means. If an object has already been created, this method either returns it or creates a new one if needed.

> **Dunder methods:** The dunder methods are special methods that start and end with double underscores. These double underscores are referred by the acronym "dunder."

# Use cases

Who would want to limit the number of instances a class has? The most common justification for this is to manage access to a shared resource, such a file or database.

# Creation of a singleton object

## Singleton in Python

The following shows the creation of a singleton object in Python. Here, `__new__` is a special or magic method (aka the dunder method). This method is invoked every time a class is instantiated. Note that the `__new__` method is invoked before the `__init__` method gets called.

> When we attempt to create an object inside of the `__new__` method in the usual way `(ClassName())`, it runs recursively. It loops back on itself until the maximum recursion depth is reached and a `RecursionError` error is thrown.

```python
class Singleton(object):
    """Represents Singleton class."""

    __instance = None

    # This __new__ method is invoked every time a class is instantiated.
    # It's invoked before the __init__ method gets called.
    def __new__(cls, *args, **kwargs):
        if not cls.__instance:
            print("Creating singleton object...")
            cls.__instance = super(Singleton, cls).__new__(cls, *args, **kwargs)
            # cls.__instance = object.__new__(cls, *args, **kwargs)  # Other way of creating an object
            # cls.__instance = Singleton() #  Will get into a RecursionError error
        return cls.__instance

def main():
    singleton_object_1 = Singleton()
    singleton_object_2 = Singleton()

if __name__ == '__main__':
    main()
```

Output:
```text
Creating singleton object...
```

The output shows that the new object is created just once.