---
title: Non-overlapping Intervals
tags: LeetCode
---

[435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

#### Solutions
1. Sort the intervals by **right** limit.
2. Loop from **left**, remove next if it overlaps with current interval. 
This is to ensure that the removed interval is "largest" interval.