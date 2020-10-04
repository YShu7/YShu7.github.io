---
title: Make Sum Divisible by P
tags: [LeetCode]
---

[1590. Make Sum Divisible by P](https://leetcode.com/problems/make-sum-divisible-by-p/)
Given an array of positive integers nums, remove the smallest subarray (possibly empty) such that the sum of the remaining elements is divisible by p. It is not allowed to remove the whole array.

Return the length of the smallest subarray that you need to remove, or -1 if it's impossible.

A subarray is defined as a contiguous block of elements in the array.

#### Solution  
[Solution](https://leetcode.com/problems/make-sum-divisible-by-p/discuss/854197/JavaC%2B%2BPython-Prefix-Sum)

1. The question is the same as 'Find the shortest array with sum % p = need.'

1. Use the distance between 2 prefix sum to get the length of the array.

```python
def minSubarray(self, nums, p):
    """
    :type nums: List[int]
    :type p: int
    :rtype: int
    """
    dp = {0: -1} # when mod is 0, no need to remove
    mod = sum(nums) % p # need remove this mod
    
    prefix_sum = 0
    res = float('inf')
    for i, n in enumerate(nums):
        prefix_sum += n
        curr_mod = prefix_sum % p
        dp[curr_mod] = i # curr remainder
        if (curr_mod - mod) % p in dp: # if needed remainder can be achieved with prefix
            res = min(res, i - dp[(curr_mod - mod) % p]) # distance between that position and curr position
    return res if res < len(nums) else -1
```