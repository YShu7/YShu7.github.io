---
title: Maximal Rectangle
tags: [LeetCode, Dynamic Programming]
---

[85. Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)
#### Solution 1
1. Convert the matrix to a list of histogram.

1. Use [84. Largest Rectangle in Histogram]()

#### Solution 2 - DP
[Solution](https://leetcode.com/problems/maximal-rectangle/discuss/29054/Share-my-DP-solution)

1. Keep the `left` and `right` for each grid, which represent its leftmost and rightmost neighbours' indexes.

1. `curr_left` and `curr_right` represents the leftmost and rightmost neighbours' indexes for **current row**.

1. We need to also consider the **previous row**, to form a rectangle, 
i.e. `left` and `right` are keeping the indexes for a particular height `height[i][j]`.

1. For each grid `matrix[i][j]`, its `h*w` is the maximal rectangle with current `height[i][j]`.
> When height is larger, the condition becomes more strict (controlled by `left` and `right`).
```
height:
[0, 0, 0, 1, 0]
[0, 0, 0, 2, 0]
[0, 0, 0, 3, 0]
[0, 0, 1, 4, 1]
[0, 0, 2, 5, 2]

left:
[0, 0, 0, 3, 0]
[0, 0, 0, 3, 0]
[0, 0, 0, 3, 0]
[0, 1, 1, 3, 0]
[0, 1, 1, 3, 0]

right:
[5, 5, 5, 4, 5]
[5, 5, 5, 4, 5]
[5, 5, 5, 4, 5]
[5, 4, 4, 4, 5]
[5, 4, 4, 4, 5]
```

```python
def maximalRectangle(self, matrix):
    """
    :type matrix: List[List[str]]
    :rtype: int
    """
    if len(matrix) == 0:
        return 0
    m, n = len(matrix), len(matrix[0])
    dp = [[0] * n for _ in range(m)]
    left = [[0] * n for _ in range(m)]
    right = [[n] * n for _ in range(m)]
    for i in range(m):
        curr_left, curr_right = 0, n
        for j in range(n):
            if matrix[i][j] == "1":
                dp[i][j] = dp[i-1][j] + 1
                left[i][j] = max(left[i-1][j], curr_left)
            elif matrix[i][j] == "0":
                curr_left = j + 1
                
            if matrix[i][n-1-j] == "1":
                right[i][n-1-j] = min(right[i-1][n-1-j], curr_right)
            elif matrix[i][n-1-j] == "0":
                curr_right = n-1-j
    res = 0
    for i in range(m):
        for j in range(n):
            res = max(res, (right[i][j] - left[i][j]) * dp[i][j])
    return res
```