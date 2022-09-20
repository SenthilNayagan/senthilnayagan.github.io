---
layout: post
title:  "Find first repeating character in a given string"
kicker: "Coding Problem Solving"
author: senthil
published_on: 2022-09-19 00:00:00 +0530
tags: ["coding-problem-solving", "coding-problem", "problem-solving"]
categories: coding-problem-solving
featured: false
hidden: true
---

# Problem: Find first repeating character in a given string

## Description

Given a string `str`, create a function that returns the first repeating character. If such character doesn't exist, return the null character `'\0'`.

### Example 1

- Input: str = "inside code"
- Output: 'i'

### Example 2

- Input: str = "programming"
- Output: 'r'

### Example 3

- Input: str = "abcd"
- Output: '\0'

<hr class="grey_line"/>

## Solution 1 (Brute force solution)

```python
# Brute force approach
def first_repeating_character(str):
    for i in range(len(str)):
        for j in range(i):
            if str[i] == str[j]:
                return str[i]
    return '\0'   

    
if __name__ == "__main__":
    print(first_repeating_character("inside code"))  # returns 'i'
    print(first_repeating_character("programming"))  # returns 'r'
    print(first_repeating_character("abcd"))  # returns '\0'
```

##### Complexity

- **Time complexity:** `O(n`<sup>`2`</sup>`)` - For each character we are traversing `str` again using both outer and inner loops.
- **Space complexity:** `O(1)` - We are using two integer variables (`i` and `j`). The extra space here is 2, which is a constant.

Let's find a better solution than this one.

<hr class="grey_line"/>

## Solution 2 (Using hash table)

This solution uses hash table. The hash table is a powerful tool when solving coding problems because it has an `O(1)` lookup on average, so we can get the value of a certain key in `O(1)`. Also, it has an `O(1)` insertion on average, so we can insert an element in `O(1)`. 

```python
def first_repeating_character(str):
    visited = {}  # Dictionary as hash table
    
    for chr in str:
        if visited.get(chr):  # O(1) for lookup
            return chr
        else:
            visited[chr] = True  # O(1) for insertion
    return '\0'
```

The below one uses index:

```python
# Using index
def first_repeating_character(str):
    visited = {}  # Dictionary as hash table
    
    for i in range(len(str)):
        if visited.get(str[i]):  # O(1) for lookup
            return str[i]
        else:
            visited[str[i]] = True  # O(1) for insertion
    return '\0'
```

### Complexity

- **Time complexity:** `O(n)` - We are fully traversing `str` once i.e., it does `n` iterations. However, the hash table lookup and insertion have an `O(1)` cost on average.
- **Space complexity:** `O(n)` - Because of the hash table we used. In the worst case, every character needs to be inserted into the hash table. This case happens when each character is unique and we don't find a repeating character. Since we have `n` characters, the extra space would be `n`.

<hr class="grey_line"/>