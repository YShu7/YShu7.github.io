---
title: Minimum Path Sum
tags: [LeetCode]
---

[64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)
#### Solution  
1. Use the grid to store the minimum path sum to the current block.

1. Consider the corner case when `i == 0 or j == 0`.

```python
def minPathSum(self, grid):
    """
    :type grid: List[List[int]]
    :rtype: int
    """
    m, n = len(grid), len(grid[0])
    start = grid[0][0]
    
    for i in range(m):
        for j in range(n):
            if i == 0 and j == 0:
                continue
            if i == 0:
                grid[i][j] += grid[i][j-1]
            elif j == 0:
                grid[i][j] += grid[i-1][j]
            else:

                grid[i][j] = min(grid[i][j] + grid[max(i-1, 0)][j], grid[i][j] + grid[i][max(j-1, 0)])
            
    return grid[m-1][n-1]
```