---
title: Binary Tree Maximum Path Sum
tags: LeetCode
---

[124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

#### Solutions
> Comparison is needed when:
> - Decide if we should update current maximum path sum.  
> - Decide left or right child can be part of the maximum path.  

1. Keep a global variable to keep track of current maximum path sum.  
2. Helper function returns maximum path sum of left / right child + current val.  
> Acting as a possible sub-path of the current maximum path.