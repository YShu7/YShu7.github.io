---
title: Maximum Sum Obtained of Any Permutation
tags: [LeetCode, Greedy]
---

[1589. Maximum Sum Obtained of Any Permutation](https://leetcode.com/problems/maximum-sum-obtained-of-any-permutation/)
We have an array of integers, nums, and an array of requests where requests[i] = [starti, endi]. The ith request asks for the sum of nums[starti] + nums[starti + 1] + ... + nums[endi - 1] + nums[endi]. Both starti and endi are 0-indexed.

Return the maximum total sum of all requests among all permutations of nums.

Since the answer may be too large, return it modulo 10^9 + 7.

#### Solution  
1. Assign the larger element to the position queried more frequently.

1. Find the counts wiht sweeping line.

```python
def maxSumRangeQuery(self, nums, requests):
    """
    :type nums: List[int]
    :type requests: List[List[int]]
    :rtype: int
    """
    l = len(nums)
    count = [0] * (l + 1)
    for start, end in requests:
        # consider it as: add all, then remove
        # mark start of a region
        # later we add all the following together
        count[start] += 1
        # mark end of a region
        # substract those not in a region
        count[end + 1] -= 1
    
    for i in range(1, len(nums)+1):
        count[i] += count[i-1]
        
    list.sort(count, reverse=True)
    list.sort(nums, reverse=True)

    res = 0
    for c, n in zip(count[:-1], nums):
        res += c * n
    return res % (10**9 + 7)
```