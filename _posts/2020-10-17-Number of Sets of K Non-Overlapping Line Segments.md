---
title: Number of Sets of K Non-Overlapping Line Segments
tags: [LeetCode, Dynamic Programming]
---

[1621. Number of Sets of K Non-Overlapping Line Segments](https://leetcode.com/problems/number-of-sets-of-k-non-overlapping-line-segments/)
Given n points on a 1-D plane, where the ith point (from 0 to n-1) is at x = i, find the number of ways we can draw exactly k non-overlapping line segments such that each segment covers two or more points. The endpoints of each segment must have integral coordinates. The k line segments do not have to cover all n points, and they are allowed to share endpoints.

Return the number of ways we can draw k non-overlapping line segments. Since this number can be huge, return it modulo 109 + 7.int getIndex(idx) Gets the current value at index idx (0-indexed) of the sequence modulo 109 + 7. If the index is greater or equal than the length of the sequence, return -1.  

#### Solution - Math  
[Solution](https://leetcode.com/problems/number-of-sets-of-k-non-overlapping-line-segments/discuss/898830/Python-O(N)-Solution-with-Prove)
1. The question is the same as: Given n + k - 1 points, take k segments, not allowed to share endpoints.

1. Easy combination number C(n + k - 1, 2k).
```python
def numberOfSets(self, n, k):
    res = 1
    for i in xrange(1, k * 2 + 1):
        res *= n + k - i
        res /= i
    return res % (10**9 + 7)
```
OR
```python
def numberOfSets(self, n, k):
    return math.comb(n + k - 1, k * 2) % (10**9 + 7)
```
#### Solution - DP  
[Solution](https://leetcode.com/problems/number-of-sets-of-k-non-overlapping-line-segments/discuss/901894/JavaPython-Top-Down-DP-Clean-and-Concise-O(4*n*k))
```python
def numberOfSets(self, n: int, k: int) -> int:
    MOD = 10**9 + 7
    @lru_cache(None)
    def dp(i, k, isStart):
        if k == 0: return 1 # Found a way to draw k valid segments
        if i == n: return 0 # Reach end of points
        ans = dp(i+1, k, isStart) # Skip ith point
        if isStart:
            ans += dp(i+1, k, False) # Take ith point as start
        else:
            ans += dp(i, k-1, True) # Take ith point as end
        return ans % MOD
    return dp(0, k, True)
```