---
layout: post
title:  "Find pair that sums up to `k`"
kicker: "Coding Problem Solving"
author: senthil
published_on: 2022-09-19 00:00:00 +0530
tags: ["coding-problem-solving", "coding-problem", "problem-solving"]
categories: coding-problem-solving
featured: false
hidden: true
---

# Problem: Find pair that sums up to "k"

## Description

Given an array of integers `nums` and an integer `k`, create a boolean function that checks if there are two elements in `nums` such that we get `k` when we add them together.

### Example 1

- Input: nums = [4, 1, 5, -3, 6], k = 11
- Output: true
- Explanation: 5 + 6 is equivalent to 11

### Example 2

- Input: nums = [4, 1, 5, -3, 6], k = -2
- Output: true
- Explanation: 1 + (-3) is equivalent to -2

<hr class="grey_line"/>

## Solution 1 (Brute force solution)

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

### Complexity
- **Time complexity:** `O(n`<sup>`2`</sup>`)`
- **Space complexity:** `O(1)`

Let's find a better solution than this one.

<hr class="grey_line"/>

## Solution 2 

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

### Complexity
- **Time complexity:** `O(n log n)`
- **Space complexity:** Depends on the sorting algorithm we use. For example, if it's `O(log n)`, then the space complexity of this algorithm is `O(long n)`.

Let's find a better solution than this one.

<hr class="grey_line"/>

## Solution 3 (Using hash table. It's the most optimal solution)

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

### Complexity

- **Time complexity:** `O(n)`
- **Space complexity:** `O(n)` - We are using additional space for a hash table that can contain `n` elements in the worst case.

The lookup and insertion are constant on average in this case. Hence, the `O(n)`. 

<hr class="grey_line"/>