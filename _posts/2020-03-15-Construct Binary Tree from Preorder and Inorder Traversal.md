---
title: Construct Binary Tree from Preorder and Inorder Traversal
tags: LeetCode
---

[105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

#### Solutions
The solution is similar to divide and conquer.  
1. Find the root by using pre-order traversal. Find the root's index in in-order traversal.  
2. Divide in-order by the index and continue.  
**Optimization**
Can use dictionary to store the index of in-order array `{num: idx}` and use iterater for pre-order traversal.