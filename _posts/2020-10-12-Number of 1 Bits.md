---
title: Number of 1 Bits
tags: [LeetCode]
---

[191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)
#### Solution  
```python
def hammingWeight(self, n):
    """
    :type n: int
    :rtype: int
    """
    res = 0
    while n != 0:
        tmp = n
        n = n >> 1
        if n << 1 != tmp:
            res += 1
            
    return res
```