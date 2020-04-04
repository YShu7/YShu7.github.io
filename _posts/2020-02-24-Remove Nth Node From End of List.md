---
title: Remove Nth Node From End of List
tags: LeetCode
---

[19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

#### Solutions
**Loop twice**
1. Loop through the list to count the length of this list
2. Loop through the list again until meets the (length - n)th node from the beginning
**Loop once**
1. Keep two pointer slow and fast, where fast is n+1 step faster than slow.
2. When fast reaches to the end, slow is pointing to the correct solution.