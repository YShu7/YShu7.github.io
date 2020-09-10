---
title: Search a 2D Matrix II
tags: [LeetCode]
---

[240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)
#### Solution 1  
1. Find a sub-matrix for searching.

1. Apply binary search to each row/col of the sub-matrix.
```python
def searchMatrix(self, matrix, target):
    """
    :type matrix: List[List[int]]
    :type target: int
    :rtype: bool
    """
    if len(matrix) == 0 or len(matrix[0]) == 0:
        return False
    
    ridx = 0
    for i, r in enumerate(matrix):
        if r[0] <= target:
            ridx = i
        else:
            break
        
    cidx = 0
    for i, c in enumerate(matrix[0]):
        if c <= target:
            cidx = i
        else:
            break

    for arr in matrix[0:ridx+1]:
        idx = bisect.bisect_left(arr[0:cidx+1], target)
        if idx >= 0 and idx < len(arr) and arr[idx] == target:
             return True
    return False
```
Complexity: O(m logn)
#### Solution 2
[Solution](https://leetcode.com/problems/search-a-2d-matrix-ii/discuss/332356/Python-O(m%2Bn)-Linear-Search-from-Top-Right-Corner)
1. Search from the top-right grid.

1. If current grid `M[r][c]` is smaller than target x, there is no need to consider `M[r][:c]` since all the grids on the left must be smaller as well. 
So, x must be in the rows below and we can safely make `r += 1`.

1. We keep moving `M[r][c]` downwards until it's larger x, then we can safely move leftwards and make `c -= 1` since all the grids in `M[r:][c]` would be larger than x.
```python
def searchMatrix(matrix, target):
	m, n = len(matrix), len(matrix) and len(matrix[0])
	r, c = 0, n-1
	while r < m and c >= 0:
		if target > matrix[r][c]:
			r += 1
		elif target < matrix[r][c]:
			c -= 1
		else: return True
	return False
```