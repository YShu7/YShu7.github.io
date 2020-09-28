---
title: Special Positions in a Binary Matrix
tags: [LeetCode]
---

[1582. Special Positions in a Binary Matrix](https://leetcode.com/problems/special-positions-in-a-binary-matrix/)
#### Solution
1. Track the sum of each row and col

1. When a row `r` and a col `c` both have `sum == 1`, check if `mat[r][c] == 1`.
If it is, this is a special position.

```python
def numSpecial(self, mat):
    """
    :type mat: List[List[int]]
    :rtype: int
    """
    m, n = len(mat), len(mat[0])
    
    rows = [sum(row) for row in mat]
    cols = [sum(col) for col in zip(*mat)]
    
    set_row, set_col = [], []
    
    for i, row in enumerate(rows):
        if row == 1:
            set_row.append(i)
    for i, col in enumerate(cols):
        if col == 1:
            set_col.append(i)
    
    res = 0
    for r in set_row:
        for c in set_col:
            if mat[r][c] == 1:
                res += 1
    
    return res
```