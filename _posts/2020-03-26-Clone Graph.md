---
title: Clone Graph
tags: LeetCode
---

[133. Clone Graph](https://leetcode.com/problems/clone-graph/submissions/)

#### Solutions
1. Maintain a node dictionary that stores all constructed node.  
2. For each new node introduced, loop through all its neighbours to find the node corresponding to the neighbour.  
> Need to use helper function.