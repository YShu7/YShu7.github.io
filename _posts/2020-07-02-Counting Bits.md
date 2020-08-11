---
title: Shortest Unsorted Continuous Subarray
tags: [LeetCode]
---

[581. Shortest Unsorted Continuous Subarray](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/)
#### Solution 1
1. Sort the array.  

1. Compare the sorted array with the original one.

#### Solution 2
1. Find the correct position of the minimum element and maximum element in the unsorted subarray.

1. To determine the correct position of min, we need to find the first element which is just larger than min.

```python
def findUnsortedSubarray(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    l = len(nums)
    local_max, local_min = nums[0], nums[l-1]
    
    start, end = -1, -2

    for i in range(1, l):

        local_max = max( local_max, nums[i] )
        local_min = min( local_min, nums[l-1-i])

        if local_max > nums[i]:
            # nums[i] < nums[i-1]
            end = i

        if local_min < nums[l-1-i]:
            # nums[size-1-i] > nums[size-i]
            start = l-1-i

    return end-start+1
```