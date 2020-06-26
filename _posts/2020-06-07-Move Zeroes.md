---
title: Move Zeroes
tags: LeetCode
---

[448. Move Zeroes](https://leetcode.com/problems/move-zeroes/)
#### Solution 1 - Swap
1. If the num is not 0, move forward if possible, i.e. `zero < i, nums[zero] = nums[i]`

1. If the num is 0, leave it and search for the number to 'fill in' it.

1. The key point is `zero < i` because `zero` is not updated at each iteration.

```python
def moveZeroes(self, nums):
    """
    :type nums: List[int]
    :rtype: None Do not return anything, modify nums in-place instead.
    """

    zero = 0  # records the position of "0"
    for i in xrange(len(nums)):
        if nums[i] != 0:
            nums[i], nums[zero] = nums[zero], nums[i]
            zero += 1
```

#### Solution 2 - Re-assign
1. Get the index of all zeros.

1. Copy the other numbers to the beginning of the array.

1. Assign the last few positions 0 value.

```python
def moveZeroes(self, nums):
    """
    :type nums: List[int]
    :rtype: void Do not return anything, modify nums in-place instead.
    """
    count=nums.count(0)
    nums[:]=[i for i in nums if i != 0]
    nums+=[0]*count
```