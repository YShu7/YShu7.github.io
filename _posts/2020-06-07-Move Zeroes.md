---
title: Find All Numbers Disappeared in an Array
tags: LeetCode
---

[448. Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
#### Solution 
1. Since the array contians number in range [1, n], we can use their value as index to track the information.

1. When a number is visited, we mark the value it is point to negative.

1. The positive number's index disappeared in the array.

[Solution](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/discuss/92955/Python-4-lines-with-short-explanation)
```python
def findDisappearedNumbers(self, nums):
    """
    :type nums: List[int]
    :rtype: List[int]
    """
    for i in xrange(len(nums)):
        index = abs(nums[i]) - 1
        nums[index] = - abs(nums[index])

    return [i + 1 for i in range(len(nums)) if nums[i] > 0]
```