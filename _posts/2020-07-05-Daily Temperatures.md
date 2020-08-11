---
title: Daily Temperatures
tags: [LeetCode]
---

[739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
#### Solution 
1. When a warmer temperature comes in, pop all temperature before and lower than it.

```python
def dailyTemperatures(self, T):
    """
    :type T: List[int]
    :rtype: List[int]
    """
    stack = []
    res = [0] * len(T)
    for i, t in enumerate(T):
        while stack and stack[-1][1] < t:
            idx, prev = stack.pop()
            res[idx] = i - idx
        stack.append((i, t))
    return res
```