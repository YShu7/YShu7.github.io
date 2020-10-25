---
title: Pascal's Triangle
tags: [LeetCode]
---

[118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/)
#### Solution  
```python
def generate(self, numRows):
    """
    :type numRows: int
    :rtype: List[List[int]]
    """
    if numRows == 0:
        return []
    res = [[1]]
    for i in range(2, numRows+1):
        row = [1] * i
        for j in range(1, i-1):
            row[j] = res[-1][j-1] + res[-1][j]
        res.append(row)
            
    return res
```