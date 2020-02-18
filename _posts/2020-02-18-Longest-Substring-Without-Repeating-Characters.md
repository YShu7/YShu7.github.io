---
title: Linked List Cycle
tags: LeetCode
---

[3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

#### Solutions
Sliding Window
1. Keep pointer `start` and `curr`, a set of `occurred` characters
2. If `s[curr]` has occurred, move `start` to next and update `occurred` **until `s[curr]` is not inside `occurred`**
3. Keep the largest `curr - start + 1` as the result