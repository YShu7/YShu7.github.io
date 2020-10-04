---
title: Game of Life
tags: [LeetCode]
---

[289. Game of Life](https://leetcode.com/problems/game-of-life/)
#### Solution  
[Solution](https://leetcode.com/problems/game-of-life/discuss/73223/Easiest-JAVA-solution-with-explanation)
1. We need to come out with an idea of keeping the current state and the next state in the matrix at the same time.

1. Use bits: `[2nd bit, 1st bit] = [next state, current state]`

1. To get the current state: `board[i][j] & 1`

1. To get the next state: `board[i][j] >> 1`

```python
def gameOfLife(self, board):
    """
    :type board: List[List[int]]
    :rtype: None Do not return anything, modify board in-place instead.
    """
    for i, row in enumerate(board):
        for j, cell in enumerate(row):
            lives = self.get(board, i, j)
            if board[i][j] == 1 and lives >= 2 and lives <= 3:
                board[i][j] = 3 # Make the 2nd bit 1: 01 ---> 11
            if board[i][j] == 0 and lives == 3:
                board[i][j] = 2 # Make the 2nd bit 1: 00 ---> 10
                
    for i, row in enumerate(board):
        for j, cell in enumerate(row):
            board[i][j] = board[i][j] >> 1
    return board
            
def get(self, board, i, j):
    res = 0
    directions = [[0, 1], [0, -1], [1, 0], [-1, 0],
                  [1, -1], [1, 1], [-1, 1], [-1, -1]]
    for d in directions:
        if i+d[0] < 0 or i+d[0] >= len(board) or \
            j+d[1] < 0 or j+d[1] >= len(board[0]):
            continue
        res += board[i+d[0]][j+d[1]] & 1
    return res
```