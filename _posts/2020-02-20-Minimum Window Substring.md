---
title: Minimum Window Substring
tags: LeetCode
---

[76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

#### Solutions
1. Create a filtered_S that contains all characters and their index that should appear in target string
2. Apply sliding window algorithm to filtered_S
Time Complexity: $O(\mid S \mid + \mid T \mid)$
Space Complexity: $O(\mid S \mid + \mid T \mid)$