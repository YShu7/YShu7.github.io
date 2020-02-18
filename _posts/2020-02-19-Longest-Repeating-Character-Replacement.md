---
title: Linked List Cycle
tags: LeetCode
---

[424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

#### Solutions
Sliding Window with tolerance
1. Keep pointer `start` and `curr`, a dictionary of `num_occurance` and `max_occurance`
2. If `curr - start + 1 - max_occurance > tolerance`  
    - Move `start` to next
    - Update `num_occurance`
    `num_occurance[ord(s[start]) - ord('A')] -= 1`
    > No need to update `max_occurance` because we only expect window ≥ `max_occurance + k`,
    thus `start` will only be updated when `max_occurance` is updated by `s[curr]`
3. return `l` or `max_occurance + tolerance`