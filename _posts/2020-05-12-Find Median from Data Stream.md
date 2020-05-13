---
title: Find Median from Data Stream
tags: LeetCode
---

[295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

#### Solutions
1. Keep 2 heap, one min_heap and one max_heap  
1. Let min_heap has at most 1 item more than max_heap  
1. The tops of two heaps will be the median of the whole list.