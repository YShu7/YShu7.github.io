---
title: Subsets
tags: [LeetCode]
---

[215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
#### Solution  
1. Randomly choose a pivot. Let all elements on the left to it smaller than it, and all elements on the right to it larger than it.

1. Return the index of the pivot after 'sorting'.

1. Select one of the partition to continue searching.

```python
def findKthLargest(self, nums, k):
    """
    :type nums: List[int]
    :type k: int
    :rtype: int
    """
    l, r, k = 0, len(nums)-1, len(nums)-k

    while True:
        mid = self.partition(nums, l, r)
        if mid < k:
            l = mid + 1
        elif mid > k:
            r = mid - 1
        else:
            return nums[mid]
            
def partition(self, nums, l, r):
    pivot_idx = randint(l, r)
    nums[pivot_idx], nums[r] = nums[r], nums[pivot_idx]

    while l < r:
        if nums[l] > nums[r]:
            nums[r], nums[l] = nums[l], nums[r]
        l += 1
    return l 
```