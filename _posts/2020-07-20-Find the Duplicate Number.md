---
title: Find the Duplicate Number
tags: [LeetCode]
---

[287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/submissions/)
#### Solution  
1. Consider the array as a linked list.

1. The existence of the duplicate number shows the existence of the cycle in the linked list.  

1. The entrance of the cycle is the duplicate number.

```python
def findDuplicate(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    slow = nums[0]
    fast = nums[nums[0]]
    
    while slow != fast:
        slow = nums[slow]
        fast = nums[nums[fast]]
        
    fast = 0
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    return slow
```