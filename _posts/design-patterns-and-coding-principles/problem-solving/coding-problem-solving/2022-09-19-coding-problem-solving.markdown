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

#### Problem:

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

<hr class="grey_line"/>

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

<hr class="grey_line"/>

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

#### Problem:

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

#### Solution 1 (Brute force solution):

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

<hr class="grey_line"/>

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

##### Complexity

- **Time complexity:** `O(n)` - We are fully traversing `str` once i.e., it does `n` iterations. However, the hash table lookup and insertion have an `O(1)` cost on average.
- **Space complexity:** `O(n)` - Because of the hash table we used. In the worst case, every character needs to be inserted into the hash table. This case happens when each character is unique and we don't find a repeating character. Since we have `n` characters, the extra space would be `n`.

<hr class="grey_line"/>

# Conclusion

In my opinion, solutions that are straightforward and simple are preferable to those that use the newest buzzword technology. Let's not get too excited about new products before discovering if they are flexible, useful, and have any adverse effects.