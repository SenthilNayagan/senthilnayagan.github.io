---
layout: post
title:  "Coding Problem Solving"
kicker: "Coding Problem Solving"
subtitle: "Coding problem solving."
image: assets/images/posts-cover-images/coding-problem-solving.jpg
author: senthil
published_on: 2022-09-19 00:00:00 +0530
tags: ["coding-problem-solving", "coding-problem", "problem-solving"]
categories: coding-problem-solving
featured: false
hidden: true
toc: true
---

# Overview

Having a talent for finding solutions to problems may accelerate our performance and put us ahead of our contemporaries or colleagues. In general, the solutions to certain problems may be accomplished in the blink of an eye, while others might take much more time, even days. As a result, it is essential to approach the programming problems in the appropriate manner in order to save time and effort. Let's not attempt to solve the problem by using a **brute-force** approach.

## Getting started with problems

### Find pair that sums up to "k"

#### Problem

Given an array of integers `nums` and an integer `k`, create a boolean function that checks if there are two elements in `nums` such that we get `k` when we add them together.

##### Example 1

- Input: nums = [4, 1, 5, -3, 6], k = 11
- Output: true
- Explanation: 5 + 6 is equivalent to 11

##### Example 2

- Input: nums = [4, 1, 5, -3, 6], k = -2
- Output: true
- Explanation: 1 + (-3) is equivalent to -2

#### Solution 1 (Brute force solution)

This solution follows brute force approach.

```python
# Brute-force approach
def find_pair(nums, k):
    for i in range(len(nums)):
        for j in range(i+1, len(nums)):
            if nums[i] + nums[j] == k:
                return True
    return False


if __name__ == "__main__":
    nums = [4, 1, 5, -3, 6]

    print(find_pair(nums, 11)) # True
    print(find_pair(nums, -4)) # False
    print(find_pair(nums, -2)) # True

```

##### Complexity
- **Time complexity:** `O(n`<sup>`2`</sup>`)`
- **Space complexity:** `O(1)`

Let's find a better solution than this one.

#### Solution 2 

This approach begins by *sorting the numbers* and then reduces the amount of traversal needed. Since the numbers are sorted in ascending order, then:

- `nums[i] >= nums[i-1]`
- `nums[i] <= nums[i+1]`

This approach uses the left index and the right index. If we increasing the left index, the `sum` value (`k`) will either increase or remain the same. In a similar manner, decreasing the right index will either bring about a reduction in the `sum` value (`k`) or cause it to stay unchanged.

```python
def find_pair(nums, k):
    nums.sort()
    left_idx = 0
    right_idx = len(nums) - 1

    while left_idx < right_idx:
        if nums[left_idx] + nums[right_idx] == k:
            return True
        elif nums[left_idx] + nums[right_idx] < k:
            left_idx += 1
        else:
            right_idx -= 1
    return False
```

##### Complexity
- **Time complexity:** `O(n log n)`
- **Space complexity:** Depends on the sorting algorithm we use. For example, if it's `O(log n)`, then the space complexity of this algorithm is `O(long n)`.

Let's find a better solution than this one.

#### Solution 3 (Using hash table)

This solution uses hash table. The hash table is a powerful tool when solving coding problems because it has an `O(1)` lookup on average, so we can get the value of a certain key in `O(1)`. Also, it has an `O(1)` insertion on average, so we can insert an element in `O(1)`. 

```python
def find_pair(nums, k):
    visited = {}  # Dictionary as hash table

    for element in nums:
        if visited.get(k - element):  # O(1) for lookup
            return True
        else:
            visited[element] = True  # O(1) for insertion
    return False
```

##### Complexity

- **Time complexity:** `O(n)`
- **Space complexity:** `O(n)` - We are using additional space for a hash table that can contain `n` elements in the worst case.

The lookup and insertion are constant on average in this case. Hence, the `O(n)`. 

<hr class="grey_line"/>

### Find first repeating character in a given string

#### Problem

Given a string `str`, create a function that returns the first repeating character. If such character doesn't exist, return the null character `'\0'`.

##### Example 1

- Input: str = "inside code"
- Output: 'i'

##### Example 2

- Input: str = "programming"
- Output: 'r'

##### Example 3

- Input: str = "abcd"
- Output: '\0'

#### Solution 1 (Brute force solution)

It's a brute force solution.

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

#### Solution 2 (Using hash table)

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

##### Complexity

- **Time complexity:** `O(n)` - We are fully traversing `str` once i.e., it does `n` iterations. However, the hash table lookup and insertion have an `O(1)` cost on average.
- **Space complexity:** `O(n)` - Because of the hash table we used. In the worst case, every character needs to be inserted into the hash table. This case happens when each character is unique and we don't find a repeating character. Since we have `n` characters, the extra space would be `n`.

<hr class="grey_line"/>

### Remove duplicates from an interger array

#### Problem

Given an array of integers `nums`, create a function that returns an array containing the values of `nums` without duplicates; the order doesn't matter.

##### Example 1

- Input: nums = [4, 2, 5, 3, 3, 1, 2, 4, 1, 5, 5, 5, 3, 1]
- Output: [4, 2, 5, 3, 1]

##### Example 2

- Input: nums = [1, 1, 1, 1, 1, 1, 1, 1]
- Output: [1]

#### Solution 1 (Brute force solution)

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

##### Complexity

- **Time complexity:** `O(n`<sup>`2`</sup>`)` - The loop is traversing elements of `nums`, so it does `n` iterations, and at each iteration, we are checking if the element is not in `output`. Note that searching for an element in an unsorted array has an `O(n)` cost. 
- **Space complexity:** `O(n)` - Because we are storing the output in a separate additional array that will contain `n` elements in the worst case, when there are no duplicates in `nums`.

Let's find a better solution than this one.

#### Solution 2 (Using sorting approach)

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
##### Complexity

- **Time complexity:** `O(n long n)` - Because we sorted the array
- **Space complexity:** `O(n)`

Let's find a better solution than this one.

#### Solution 3 (Using hash table and without the need for sorting)

This solution uses hash table. The hash table is a powerful tool when solving coding problems because it has an `O(1)` lookup on average, so we can get the value of a certain key in `O(1)`. Also, it has an `O(1)` insertion on average, so we can insert an element in `O(1)`. Also, this solution does not require the input data to be sorted.

```python
def remove_duplicates(nums):
    visited = {}  # Dictionary as hash table

    for element in nums:  # This iterates n times though!
        visited[element] = True  # Overwrites the already present elements
    return list(visited.keys())
```

##### Complexity

- **Time complexity:** `O(n)` - Because we are traversing completely during worst case.
- **Space complexity:** `O(n)` - Because of the hash map.

<hr class="grey_line"/>

### Find the duplicate

#### Problem

Given an array of integers `nums` that contains `n+1` elements between `1` and `n` inclusive, create a function that returns the duplicate element (the element that appears more than once).

##### Assumptions:

- There is only one duplicate number.
- The duplicate number can be repeated more than once.

> **Pigeonhole principle:** The pigeonhole principle states that if `n` items are put into `m` containers, with `n > m`, then at least one container must contain more than one item. In other words, at least 2 items share the same container. In this problem case, at least two elements have the same value i.e., duplicate values.

##### Example 1

- Input: nums = [4, 2, 1, 3, 1]
- Output: 1

##### Example 2

- Input: nums = [1, 4, 2, 2, 5, 2]
- Output: 2

#### Solution 1 (Brute force solution)

It's a brute force solution.

```python
# Brute force approach
def find_duplicate(nums):
    for i in range(len(nums)):
        for j in range(i+1, len(nums)):
            if nums[i] == nums[j]:
                return nums[i]

if __name__ == "__main__":
    print(find_duplicate([4, 2, 1, 3, 1]))  # Returns 1
    print(find_duplicate([1, 4, 2, 2, 5, 2]))  # Returns 2
```

##### Complexity

- **Time complexity:** `O(n`<sup>`2`</sup>`)`
- **Space complexity:** `O(1)`

Let's find a better solution than this one.

#### Solution 2 (Sorting approach)

This solution uses sorting approach.

```python
def find_duplicate(nums):
    nums.sort()
    
    for i in range(1, len(nums)):
        if nums[i] == nums[i-1]:
            return nums[i]
```

##### Complexity

- **Time complexity:** `O(n log n)`
- **Space complexity:** Depends on the sorting algorithm we use. For example, if it's `O(log n)`, then the space complexity of this algorithm is `O(log n)`.

Let's find a better solution than this one. 

#### Solution 3 (Using hash table)

This solution uses hash table.

```python
def find_duplicate(nums):
    visited = {}  # Dictionary as hash table

    for element in nums:
        if visited.get(element):
            return element
        else:
            visited[element] = True
    return False
```
##### Complexity

- **Time complexity:** `O(n)` - Because we traversing the array once i.e., `n` times.
- **Space complexity:** `O(n)` - Due to hash table.

Let's find a better solution than this one.

#### Solution 4 (Using floyd's cycle detection)

> **Floyd's cycle detection or tortoise and hare algorithm:**<br/>Floyd's cycle detection method is a pointer algorithm that employs just two pointers, going through the sequence at various speeds. It is also known as the tortoise and the hare algorithm. The objective of this algorithm is to establish whether or not the linked list contains a cycle.<br/><br/>First, we keep two pointers to the head node. At each iteration, we will move one of the points forward by two steps, while the other pointer will advance by one step. So we have two pointers, the tortoise and the hare. In practice, the tortoise is able to pull ahead by one distance unit, but the hare eventually gets within two distance units of it.

> **Note:** The below code throws array out of bound exception if any of the element value goes beyond the length of the array.

```python
def find_duplicate(nums):
    tortoise = nums[0]
    hare = nums[nums[0]]

    while tortoise != hare:
        tortoise = nums[tortoise]  # Is the index of the next node
        hare = nums[nums[hare]]  # Moves by 2 nodes. It's the index of other node that comes after the next one
    tortoise = 0  # Goes back to where it starts

    while tortoise != hare:
        tortoise = nums[tortoise]
        hare = nums[hare]
    return tortoise
```
##### Complexity

- **Time complexity:** `O(n)` 
- **Space complexity:** `O(1)`

<hr class="grey_line"/>

### Tree depth first search

#### Problem

Given a binary tree of integers `root`, create 3 functions to print the tree nodes in preorder, inorder, and postorder traversal.

**Preorder:** Print the root value, then print the left subtree, then print the right subtree.
**Inorder:** Print the left subtree, then print the root value, then print the right subtree.
**Postorder:** Print the left subtree, then print the right subtree, then print the root value.

It is important to keep in mind that the tree is not a linear data structure like an array or a linked list. Array has a starting point (first element), and an ending point (last element). To traverse an array, we can just start at the first element and move straightforwardly until the last element. 

This, however, is not the case with trees, even if we do have a starting point, which is the root. Therefore, we need to figure out a means to go through all of the nodes in a tree. We have two main ways to do it:

1. Do it via **BFS** (Breadth First Search) - We traverse the tree level-by-level, starting from the root towards the bottom.
2. DO it via **DFS** (Depth First Search) - 

<hr class="grey_line"/>

# Conclusion

In my opinion, solutions that are straightforward and simple are preferable to those that use the newest buzzword technology. Let's not get too excited about new products before discovering if they are flexible, useful, and have any adverse effects.