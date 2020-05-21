---
title: Longest Consecutive Sequence
tags: LeetCode
---

[128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

#### Solution
1. Find the smallest number in each sequence  
2. Get the length of the sequence by counting the number of larger numbers in the sequence  
Complexity: O(n)
```python
def longestConsecutive(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    set_nums = set(nums)
    res = 0
    
    for num in nums:
        if num - 1 in set_nums:
            continue
        
        length = 1
        end = num
        while end + 1 in set_nums:
            length += 1
            end += 1
        res = max(length, res)
    return res
```