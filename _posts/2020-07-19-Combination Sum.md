---
title: Combination Sum
tags: [LeetCode]
---

[39. Combination Sum](https://leetcode.com/problems/combination-sum/)
#### Solution  
1. Note that one number can be used for multiple times.

1. Once a number is greater than the target, it is confirmed that it cannot be used anymore. We should exclude it.

1. Otherwise, continue using the same set and check `target - c`.

```python
def combinationSum(self, candidates, target):
    """
    :type candidates: List[int]
    :type target: int
    :rtype: List[List[int]]
    """
    res = []
    if target == 0:
        return [[]]
    for i, c in enumerate(candidates):
        if c > target:
            continue
        subres = self.combinationSum(candidates[i:], target - c)
        for sr in subres:
            res.append([c] + sr)
    return res
```