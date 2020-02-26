---
title: Palindromic Substrings
tags: LeetCode
---

[647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)

#### Solutions
**Solution 1:**
For each char in the string, there are 2 kinds of palindromic substring possible:
1. odd substring
2. even substring
Expand from each char as a center for both odd and even substring.
**Solution 2:** Manacher's Algorithm