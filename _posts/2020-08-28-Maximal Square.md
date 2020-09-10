---
title: Maximal Square
tags: [LeetCode, Dynamic Programming]
---

[221. Maximal Square](https://leetcode.com/problems/maximal-square/)
#### Solution
[Solution](https://leetcode.com/problems/maximal-square/discuss/600149/Python-Thinking-Process-Diagrams-DP-Approach)
1. Keep the length of the square instead of the area.

1. The length of a square depends on the 3 girds which are neighbors to it. The smallest decides its length.
```python
def maximalSquare(self, matrix):
    """
    :type matrix: List[List[str]]
    :rtype: int
    """
    if matrix is None or len(matrix) == 0:
        return 0
    
    res = 0
    prev = [0] * (len(matrix[0]) + 1)
    for i in range(len(matrix)):
        curr = [0] * (len(matrix[0]) + 1)
        for j in range(len(matrix[0])):
            if matrix[i][j] == '1':
                curr[j+1] = min(curr[j], prev[j+1], prev[j]) + 1
                res = max(res, curr[j+1])
        prev = curr
    return res ** 2
```