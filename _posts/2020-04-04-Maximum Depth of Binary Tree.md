---
title: Maximum Depth of Binary Tree
tags: LeetCode
---

[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

#### Solutions
**Solution 1**  
Recursively get `1 + max(left, right)`  
**Solution 2**  
Use stack to keep track of node and its level.  
Use a variable to keep `max_level`, update it each time reaching a leaf.