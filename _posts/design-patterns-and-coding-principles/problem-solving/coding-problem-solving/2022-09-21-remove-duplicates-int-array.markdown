---
layout: post
title:  "Remove duplicates from an integer array"
kicker: "Coding Problem Solving"
author: senthil
published_on: 2022-09-21 00:00:00 +0530
tags: ["coding-problem-solving", "coding-problem", "problem-solving"]
categories: coding-problem-solving
featured: false
hidden: true
---

# Problem: Remove duplicates from an integer array

## Description

Given an array of integers `nums`, create a function that returns an array containing the values of `nums` without duplicates; the order doesn't matter.

### Example 1

- Input: nums = [4, 2, 5, 3, 3, 1, 2, 4, 1, 5, 5, 5, 3, 1]
- Output: [4, 2, 5, 3, 1]

### Example 2

- Input: nums = [1, 1, 1, 1, 1, 1, 1, 1]
- Output: [1]

<hr class="grey_line"/>

## Solution 1 (Brute force solution)

In this brute force solution we create an empty array, `output`. For each element of `nums`, we check if we didn't put in the `output` yet. If it's the case, we push it, and we continue.

```python
# Brute force approach
def remove_duplicates(nums):
    output = []

    for element in nums:
        if element not in output:
            output.append(element)
    return output


if __name__ == "__main__":
    print(remove_duplicates([4, 2, 5, 3, 3, 1, 2, 4, 1, 5, 5, 5, 3, 1]))  # returns [4, 2, 5, 3, 1]
    print(remove_duplicates([1, 1, 1, 1, 1, 1, 1, 1]))  # returns [1]
```

### Complexity

- **Time complexity:** `O(n`<sup>`2`</sup>`)` - The loop is traversing elements of `nums`, so it does `n` iterations, and at each iteration, we are checking if the element is not in `output`. Note that searching for an element in an unsorted array has an `O(n)` cost. 
- **Space complexity:** `O(n)` - Because we are storing the output in a separate additional array that will contain `n` elements in the worst case, when there are no duplicates in `nums`.

Let's find a better solution than this one.

<hr class="grey_line"/>

## Solution 2 (Using sorting approach)

It's a sorting approach.

```python
def remove_duplicates_sorted(nums):
    if len(nums) == 0:
        return []
    
    nums.sort()
    output = [nums[0]]

    for i in range(1, len(nums)):
        if nums[i] != nums[i-1]:
            output.append(nums[i])
    return output
```
### Complexity

- **Time complexity:** `O(n long n)` - Because we sorted the array
- **Space complexity:** `O(n)`

Let's find a better solution than this one.

<hr class="grey_line"/>

## Solution 3 (Using hash table and without the need for sorting)

This solution uses hash table. The hash table is a powerful tool when solving coding problems because it has an `O(1)` lookup on average, so we can get the value of a certain key in `O(1)`. Also, it has an `O(1)` insertion on average, so we can insert an element in `O(1)`. Also, this solution does not require the input data to be sorted.

```python
def remove_duplicates(nums):
    visited = {}  # Dictionary as hash table

    for element in nums:  # This iterates n times though!
        visited[element] = True  # Overwrites the already present elements
    return list(visited.keys())
```

### Complexity

- **Time complexity:** `O(n)` - Because we are traversing completely during worst case.
- **Space complexity:** `O(n)` - Because of the hash map.

<hr class="grey_line"/>