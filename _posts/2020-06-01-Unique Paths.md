---
title: Unique Paths
tags: LeetCode [Dynamic Programming]
---

[62. Unique Paths](https://leetcode.com/problems/unique-paths/)
#### Solution 1
1. For each box, there are two possible directions to approach it (from **left** or from **top**).  
1. Sum the two approaches together for the final result.  
Space Complexity: O(m * n)
#### Solution 2
1. For each box, there are two possible directions to approach it (from **left** or from **top**).  
1. We store the number of unique paths for the last row we are traversing, s.t. we do not have to store the information 
of previous rows.  
Space Complexity: O(n)