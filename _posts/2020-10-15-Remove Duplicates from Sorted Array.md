---
title: Remove Duplicates from Sorted Array
tags: [LeetCode]
---

[26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
#### Solution  
```python
def removeDuplicates(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    pre_idx = 0
    
    i = 1
    while i < len(nums):
        if nums[pre_idx] != nums[i]:
            nums[pre_idx+1] = nums[i]
            pre_idx += 1
        i += 1
            
    return pre_idx + 1
```