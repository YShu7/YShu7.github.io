---
title: Maximum Subarray
tags: [LeetCode, Dynamic Programming], [Kadane's Algorithm]
---

[53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
#### Solution 
1. Consider the sequence as family, the previous one is the next one's father.  
2. To make the last child have most money, we will only inherit wealth but not debt.  
3. If the father has sum > 0, the child get it, otherwise the child restart his own life.  
```python
def maxSubArray(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    curr_max = nums[0]
    res = curr_max
    for i, num in enumerate(nums[1:]):
        curr_max = max(num, curr_max + num)
        res = max(curr_max, res)
    return res
```