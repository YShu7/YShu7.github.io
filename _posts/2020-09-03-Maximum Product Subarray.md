---
title: Maximum Product Subarray
tags: [LeetCode]
---

[152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)
#### Solution 
1. Only when the num is 0, we are sure to restart.

1. Choose the even number of negative nums from left and right, compare them.

```python
def maxProduct(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    res = nums[0]
    curr = 1
    
    for num in nums:
        curr = curr * num
        res = max(res, curr)
        if curr == 0:
            curr = 1
            
    curr = 1
    for num in reversed(nums):
        curr = curr * num
        res = max(res, curr)
        if curr == 0:
            curr = 1
    return res
```