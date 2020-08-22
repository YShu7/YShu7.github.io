---
title: Subarray Sum Equals K
tags: [LeetCode]
---

[560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)
#### Solution  
[Solution](https://leetcode.com/problems/subarray-sum-equals-k/discuss/341399/Python-clear-explanation-with-code-and-example)

1. Whenever the sums has increased by a value of k, we've found a subarray of sums=k.

1. 0 is needed as the initial value to find the first subarray.

```python
def subarraySum(self, nums, k):
    """
    :type nums: List[int]
    :type k: int
    :rtype: int
    """
    sums = 0
    res = 0
    dic = collections.Counter({0: 1})
    for i, num in enumerate(nums):
        sums += num
        res += dic[sums - k]
        dic[sums] += 1
    return res
```