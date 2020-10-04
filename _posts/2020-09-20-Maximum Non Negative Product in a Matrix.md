---
title: Maximum Non Negative Product in a Matrix
tags: [LeetCode, Dynamic Programming]
---

[1594. Maximum Non Negative Product in a Matrix](https://leetcode.com/problems/maximum-non-negative-product-in-a-matrix/)

You are given a rows x cols matrix grid. Initially, you are located at the top-left corner (0, 0), and in each step, you can only move right or down in the matrix.

Among all possible paths starting from the top-left corner (0, 0) and ending in the bottom-right corner (rows - 1, cols - 1), find the path with the maximum non-negative product. The product of a path is the product of all integers in the grid cells visited along the path.

Return the maximum non-negative product modulo 109 + 7. If the maximum product is negative return -1.

Notice that the modulo is performed after getting the maximum product.

#### Solution  
1. Use two dp to store the previous result. One for positive and one for negative.

1. Update the two matrix according to the fact if `grid[i][j]` is positive or not.

```python
def maxProductPath(self, grid):
    """
    :type grid: List[List[int]]
    :rtype: int
    """
    m, n = len(grid), len(grid[0])
    dp = [[[0, 0] for _ in range(n)] for _ in range(m)]
    dp[0][0] = [grid[0][0], grid[0][0]]
    
    for i in range(1,m):
        dp[i][0][0] = dp[i-1][0][0] * grid[i][0]
        dp[i][0][1] = dp[i][0][0]
    for j in range(1, n):
        dp[0][j][0] = dp[0][j-1][0] * grid[0][j]
        dp[0][j][1] = dp[0][j][0]

    for i in range(1, m):
        for j in range(1, n):
            if grid[i][j] < 0:
                dp[i][j][0] = (min(dp[i-1][j][1], dp[i][j-1][1]) * grid[i][j]);
                dp[i][j][1] = (max(dp[i-1][j][0], dp[i][j-1][0]) * grid[i][j]);
            else:
                dp[i][j][1] = (min(dp[i-1][j][1], dp[i][j-1][1]) * grid[i][j]);
                dp[i][j][0] = (max(dp[i-1][j][0], dp[i][j-1][0]) * grid[i][j]);

    res = dp[-1][-1][0]
    return res  % (10 ** 9 + 7) if res >= 0 else -1
```