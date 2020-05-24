---
title: Climbing Stairs
tags: LeetCode, [Dynamic Programming]
---

[70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

#### Solution
1. Keep the number of distinct ways to climb `n` stairs.  
2. `dict[n] = dict[n-1] + dict[n-2]`  
Similar to induction, base case: `n = 1` and `n = 2`