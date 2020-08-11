---
title: Kth Largest Element in an Array
tags: [LeetCode]
---

[215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/submissions/)
#### Solution  
1. Use divide and conquer.

1. Change the problem to Kth smallest element in the array.

1. Randomly select the pivot can make the process faster.

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