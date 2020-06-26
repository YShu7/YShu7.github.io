---
title: House Robber
tags: [LeetCode, Dynamic Programming]
---

[198. House Robber](https://leetcode.com/problems/house-robber/)
[213. House Robber II](https://leetcode.com/problems/house-robber-ii/)
#### Solution
If we rob the `curr` house, we cannot rob `curr-1` house.   
1. Keep `max_2_house_before` as the amount we can get if we do NOT rob `curr-1`, i.e. we can rob `curr`.  
1. Keep `adjacent` as the amount we can get if we rob `curr-1`, i.e. we cannot rob `curr`.  
1. Althouge we **can** rob `curr`, we do NOT necessarily rob it. We can leave 2 house not robbed to get better result. 
In this case, we keep `adjacent` instead of `max_2_house_before + curr`.  
Complexity: O(n)
```python
def rob(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    max_2_house_before, adjacent = 0, 0
    for cur in nums:
        max_2_house_before, adjacent = \
        adjacent, max(adjacent, max_2_house_before+cur)
    return max(max_2_house_before, adjacent)
```
When the houses are in a circle. We can only rob one of the first and the last house. 
It is the same as rob `nums[1:]` and `nums[:-1]`.