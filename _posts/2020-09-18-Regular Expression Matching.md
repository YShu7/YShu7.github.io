---
title: Regular Expression Matching
tags: [LeetCode, Dynamic Programming]
---

[10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)
#### Solution  
[Solution](https://leetcode.com/problems/regular-expression-matching/discuss/5651/Easy-DP-Java-Solution-with-detailed-Explanation)

1. `dp[i][j]` represents if `p[j-1]` and `s[i-1]` can match each other.

1. When `s` is empty, if all previous even indexs of `p` are `*`, they can match. Otherwise, cannot.

1. When `p[j-1] == '*'`

    if `pre_p == curr_s`:  
        a* counts as multiple a: `dp[i][j] = dp[i-1][j]`  
        a* counts as single a: `dp[i][j] = dp[i][j-1]`  
        a* counts as empty: `dp[i][j] = dp[i][j-2]`  
        
    else:
        a* counts as empty: `dp[i][j] = dp[i][j-2]`  
        
1. When `p[j-1] == s[i-1]`: `dp[i][j] = dp[i-1][j-1]`

```python
def isMatch(self, s, p):
    """
    :type s: str
    :type p: str
    :rtype: bool
    """
    M, N = len(s), len(p)
    dp = [[False] * (N+1) for _ in range(M+1)]
    
    dp[0][0] = True
    for n in range(1, N, 2):
        if p[n] == '*':
            dp[0][n+1] = True
        else:
            break

    for m in range(1, M+1):
        for n in range(1, N+1):
            curr_p, pre_p = p[n-1], p[n-2]
            curr_s = s[m-1]
            if curr_p == '*':
                if pre_p == '.' or pre_p == curr_s:
                    # multiple, zero, one
                    dp[m][n] = dp[m-1][n] or dp[m][n-2] or dp[m-1][n-2]
                else:
                    dp[m][n] = dp[m][n-2]
            elif curr_p == '.' or curr_p == curr_s:
                dp[m][n] = dp[m-1][n-1]
    return dp[-1][-1]            
```