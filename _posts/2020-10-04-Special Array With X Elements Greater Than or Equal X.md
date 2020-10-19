---
title: Special Array With X Elements Greater Than or Equal X
tags: [LeetCode]
---

[1608. Special Array With X Elements Greater Than or Equal X](https://leetcode.com/problems/special-array-with-x-elements-greater-than-or-equal-x/)
You are given an array nums of non-negative integers. nums is considered special if there exists a number x such that there are exactly x numbers in nums that are greater than or equal to x.

Notice that x does not have to be an element in nums.

Return x if the array is special, otherwise, return -1. It can be proven that if nums is special, the value for x is unique.

#### Solution  
1. Sort the arr. `x` falls in `[0, len(nums)]`.

1. If `x <= nums[i]` and `x > nums[i]`, means there're exactly `x` numbers >= `x`.

```python
def specialArray(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    list.sort(nums)
    l = len(nums)
    
    pre = -1
    for i, num in enumerate(nums):
        if (l - i) <= num and (l - i) > pre:
            return l - i
        pre = num
    return -1
```