---
title: Lowest Common Ancestor of a Binary Search Tree
tags: LeetCode
---

[235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

#### Solutions
Use the definition of BST. The ancestor can only be updated when `p` and `q` are in the same subtree of the current node.