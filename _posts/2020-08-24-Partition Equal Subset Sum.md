---
title: Partition Equal Subset Sum
tags: [LeetCode, Dynamic Programming]
---

[416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)
#### Solution 1  
1. If the sum `sum` of the array is odd, it cannot be partitioned.

1. If any element in the array is greater than `target = sum / 2`, it cannot be partitioned.

1. If `target` is in the array, it can be partitioned directly.

1. Otherwise try with (a) keep the first num or (b) discard the first num from the list.
> Sort the array in a reversed order may make this process faster, 
> because looking at the larger num first can deal with early breaking.

```python
def canPartition(self, nums):
    """
    :type nums: List[int]
    :rtype: bool
    """
    target = reduce(lambda x,y: x+y, nums)
    if target % 2 != 0:
        return False
    
    return self.helper(target / 2, sorted(nums, reverse=True))
    
def helper(self, target, nums):
    if len(nums) == 0:
        return False
    
    if target < 0:
        return False
    
    dic = set()
    for num in nums:
        if num > target:
            return False
        dic.add(num)
        
    if target in dic:
        return True
    
    return self.helper(target - nums[0], nums[1:]) or self.helper(target, nums[1:])
```
#### Solution 2 - 0/1 knapsack
[Solution](https://leetcode.com/problems/partition-equal-subset-sum/discuss/90592/01-knapsack-detailed-explanation)