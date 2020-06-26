---
title: Combination Sum IV
tags: LeetCode [Dynamic Programming]
---

[377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/)

#### Solution - Memoization
```python
def combinationSum4(self, nums, target):
    """
    :type nums: List[int]
    :type target: int
    :rtype: int
    """
    if target == 0:
        return 1
    if target < 0:
        return 0
    
    res = 0
    for num in nums:
        if target - num in self.dict:
            res += self.dict[target - num]
        else:
            res += self.combinationSum4(nums, target - num)
    self.dict[target] = res
    return res
```
```python
def combinationSum4(self, nums: List[int], target: int) -> int:

	# in our dp table, element i represents if target = i
	dp = [0] * (target+1) # create table that is 1 larger than target 
	dp[0] = 1 # the reason why we want +1 is because the first element is the "base case"

	for i in range(1, target+1): # offset from 1 to skip base case (first element)
		for n in nums:
			if n <= i: # if n > i, then i-n < 0, leading to index out of bounds
				dp[i] += dp[i-n] # sum all entries that are n (elements of nums) distance away

	# return the last element of dp table, since it represents the target passed in
	return dp[-1]
```