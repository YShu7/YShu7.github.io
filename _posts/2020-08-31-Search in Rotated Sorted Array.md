---
title: Search in Rotated Sorted Array
tags: [LeetCode]
---

[33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
#### Solution - Modified Binary Search
1. In this case we cannot 'throw away' half of the array like in binary search, because the array is rotated.

1. We try if the 1st half of the array works, if it works, we're done, otherwise try with another half.
```python
def search(self, nums, target):
    """
    :type nums: List[int]
    :type target: int
    :rtype: int
    """
    return self.helper(nums, target, 0, len(nums))
    
def helper(self, nums, target, l, r):
    mid = (l+r) // 2
    if l > r or mid >= len(nums) or mid < 0:
        return -1
    if l == r and nums[mid] != target:
        return -1
    if nums[mid] == target:
        return mid

    lr = self.helper(nums, target, mid+1, r)
    if lr != -1:
        return lr
    return self.helper(nums, target, l, mid)
```