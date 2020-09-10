---
title: Find First and Last Position of Element in Sorted Array
tags: [LeetCode]
---

[34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
#### Solution 1 - Use Library
```python
def searchRange(self, nums, target):
    """
    :type nums: List[int]
    :type target: int
    :rtype: List[int]
    """
    if len(nums) == 0:
        return [-1, -1]
    l, r = min(len(nums) - 1, bisect.bisect_left(nums, target)), bisect.bisect_right(nums, target) - 1
    res = [-1, -1]
    if nums[l] == target:
        res[0] = l
    if nums[r] == target:
        res[1] = r
    return res
```
#### Solution 2
[Solution](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/discuss/14714/16-line-Python-solution-symmetric-and-clean-binary-search-52ms)
```python
def searchRange(self, nums, target):
    def binarySearchLeft(A, x):
        left, right = 0, len(A) - 1
        while left <= right:
            mid = (left + right) / 2
            if x > A[mid]: left = mid + 1
            else: right = mid - 1
        return left

    def binarySearchRight(A, x):
        left, right = 0, len(A) - 1
        while left <= right:
            mid = (left + right) / 2
            if x >= A[mid]: left = mid + 1
            else: right = mid - 1
        return right
        
    left, right = binarySearchLeft(nums, target), binarySearchRight(nums, target)
    return (left, right) if left <= right else [-1, -1]
```