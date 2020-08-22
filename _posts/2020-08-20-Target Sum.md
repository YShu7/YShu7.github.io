---
title: Target Sum
tags: [LeetCode]
---

[494. Target Sum](https://leetcode.com/problems/target-sum/)
#### Solution  
1. Store sums and # ways up to `nums[i-1]` into a dictionary `dic`.

1. For `nums[i]`, create a new dictionary.

1. For each sum in `dic` up to `nums[i-1]`, `sum+k` and `sum-k` can be created in `dic[sum]` **MORE** ways.
> This is because `sum1-k` can be equal `sum2+k`.

```python
def findTargetSumWays(self, nums, S):
    """
    :type nums: List[int]
    :type S: int
    :rtype: int
    """
    dic = collections.Counter({0:1})
    
    for num in nums:
        new_dic = collections.Counter()
        for k in dic:
            if k in dic:
                new_dic[k + num] += dic[k]
                new_dic[k - num] += dic[k]
        dic = new_dic
    return dic[S]
```