---
title: Perfect Squares
tags: [LeetCode]
---

[279. Perfect Squares](https://leetcode.com/problems/perfect-squares/)
#### Solution  
1. `_dp` stores the least number of perfect squares for `index`.

1. When `len(_dp) < n`, we can update it using the previous results.

1. For `i`, it can use results from `_dp[i - perfect squares] + 1`.

1. Select the minimum of all results, it is the result for `_dp[i]`.

```python
_dp = [0]
        
def numSquares(self, n):
    """
    :type n: int
    :rtype: int
    """
    
    dp = self._dp
    while len(dp) <= n:
        dp += min(dp[-i*i] for i in range(1, int(len(dp)**0.5+1))) + 1,
    return dp[n]
```