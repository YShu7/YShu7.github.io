---
title: Kth Smallest Element in a BST
tags: LeetCode
---

[230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

#### Solutions - In-order Traversal
1. A helper function to traverse the tree in order. 
The function will be popped up from the stack, and thus its result is reversed.
2. `count--` for each 'root' node.
3. When `count == 0`, we have found the result.