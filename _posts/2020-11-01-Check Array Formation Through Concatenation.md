---
title: Check Array Formation Through Concatenation
tags: [LeetCode]
---

[1640. Check Array Formation Through Concatenation](https://leetcode.com/problems/check-array-formation-through-concatenation/)
You are given an array of distinct integers arr and an array of integer arrays pieces, where the integers in pieces are distinct. Your goal is to form arr by concatenating the arrays in pieces in any order. However, you are not allowed to reorder the integers in each array pieces[i].

Return true if it is possible to form the array arr from pieces. Otherwise, return false.

The integers in arr are distinct.  
The integers in pieces are distinct (i.e., If we flatten pieces in a 1D array, all the integers in this array are distinct).  

#### Solution  
1. Note that all integers in arr and pieces are distinct.

1. We can use map directly.

```python
def canFormArray(self, arr: List[int], pieces: List[List[int]]) -> bool:
    mp = {x[0]: x for x in pieces}
    res = []
    
    for num in arr:
        res += mp.get(num, [])
        
    return res == arr
```