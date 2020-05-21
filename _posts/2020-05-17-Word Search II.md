---
title: Word Search II
tags: LeetCode
---

[212. Word Search II](https://leetcode.com/problems/word-search-ii/)

#### Solution
1. Use `Trie` to store all searched words.  
1. `dfs` across the board. If the node is inside `Trie` and is marked as `isWord`, 
it means it is a searched word and we can traverse the board to get it.
> Remove visited node in the board by marking them as `#`
```python
class TireNode():
    def __init__(self):
        self.children = collections.defaultdict(TireNode)
        self.isWord = False
class Tire():
    def __init__(self):
        self.root = TireNode()
    
    def insert(self, word):
        node = self.root
        for w in word:
            node = node.children[w]
        node.isWord = True
        
class Solution(object):
    def findWords(self, board, words):
        """
        :type board: List[List[str]]
        :type words: List[str]
        :rtype: List[str]
        """
        res = []
        tire = Tire()
        node = tire.root
        
        for word in words:
            tire.insert(word)
    
        for row in range(len(board)):
            for col in range(len(board[0])):
                self.dfs(board, node, row, col, "", res)
        return res
    
    def dfs(self, board, node, row, col, path, res):
        directions = [[-1, 0], [1, 0], [0, 1], [0, -1]]
        if node.isWord:
            res.append(path)
            node.isWord = False
        if (row >= len(board) or row < 0 or col >= len(board[0]) or col < 0):
            return
        
        tmp = board[row][col]
        node = node.children.get(tmp)
        if not node:
            return 
        board[row][col] = "#"
        for direction in directions:
            self.dfs(board, node, row + direction[0], col + direction[1], path+tmp, res)
        board[row][col] = tmp
```