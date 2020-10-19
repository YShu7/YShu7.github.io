---
title: Find Valid Matrix Given Row and Column Sums
tags: [LeetCode]
---

[1605. Find Valid Matrix Given Row and Column Sums](https://leetcode.com/contest/biweekly-contest-36/problems/find-valid-matrix-given-row-and-column-sums/)
You are given two arrays rowSum and colSum of non-negative integers where rowSum[i] is the sum of the elements in the ith row and colSum[j] is the sum of the elements of the jth column of a 2D matrix. In other words, you do not know the elements of the matrix, but you do know the sums of each row and column.

Find any matrix of non-negative integers of size rowSum.length x colSum.length that satisfies the rowSum and colSum requirements.

Return a 2D array representing any matrix that fulfills the requirements. It's guaranteed that at least one matrix that fulfills the requirements exists.

#### Solution 1   
1. Generate the initial matrix which satisfy the `rowSum`.

1. Move the values forward or backward to fit in the `colSum`.

```python
def restoreMatrix(self, rowSum, colSum):
    """
    :type rowSum: List[int]
    :type colSum: List[int]
    :rtype: List[List[int]]
    """
    self.R, self.C = len(rowSum), len(colSum)
    self.res =[[0] * self.C for _ in range(self.R)]
    
    for i, row in enumerate(self.res):
        self.res[i][0] = rowSum[i]
    # print(self.res)
    for j, cs in enumerate(colSum):
        currCol = sum([self.res[i][j] for i in range(self.R)])
        # print(currCol, cs)
        if currCol > cs:
            self.moveToAfterwords(j, currCol, cs)
        elif currCol < cs:
            self.getFromAfterwords(j, currCol, cs)
        else:
            continue
        
    return self.res
            
def moveToAfterwords(self, colIdx, currCol, cs):
    for j in range(colIdx+1, self.C):
        for i in range(self.R):
            if self.res[i][colIdx] > 0:
                tmp = min(self.res[i][colIdx], currCol - cs)
                self.res[i][colIdx] -= tmp
                self.res[i][j] += tmp
                currCol -= tmp
                # print(self.res)
                if currCol - cs == 0:
                    return
def getFromAfterwords(self, colIdx, currCol, cs):
    for j in range(colIdx+1, self.C):
        for i in range(self.R):
            if self.res[i][j] > 0:
                tmp = min(self.res[i][j], cs - currCol)
                self.res[i][colIdx] += tmp
                self.res[i][j] -= tmp
                currCol += tmp
                if currCol - cs == 0:
                    return
```
#### Solution 2 - Greedy  
[Solution](https://leetcode.com/problems/find-valid-matrix-given-row-and-column-sums/discuss/876845/JavaC%2B%2BPython-Easy-and-Concise-with-Prove)

1. For each result value at `A[i][j]`, we greedily take the `min(row[i], col[j])`.

1. Then we update the row sum and col sum: `row[i] -= A[i][j]`, `col[j] -= A[i][j]`
```python
# Time O(mn)
# Space O(mn)
def restoreMatrix(self, row, col):
    m, n = len(row), len(col)
    A = [[0] * n for i in xrange(m)]
    for i in xrange(m):
        for j in xrange(n):
            A[i][j] = min(row[i], col[j])
            row[i] -= A[i][j]
            col[j] -= A[i][j]
    return A
```
```java
# Time O(mn) for initializing output
# Time O(m + n) for process
# Space O(mn)
public int[][] restoreMatrix(int[] row, int[] col) {
    int m = row.length, n = col.length, i = 0, j = 0, a;
    int[][] A = new int[m][n];
    while (i < m && j < n) {
        a = A[i][j] = Math.min(row[i], col[j]);
        if ((row[i] -= a) == 0) ++i;
        if ((col[j] -= a) == 0) ++j;
    }
    return A;
}
```