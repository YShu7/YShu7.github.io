---
title: Diameter of Binary Tree
tags: LeetCode, [Tree], [DFS & BFS]
---

[543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)
#### Solution 
1. The same as 1 + left tree's height + right tree's height
1. Keep a variable to track the current longest path
1. As we checking each node's height, the variable will get updated and store the maximum value in the end.
```python
def diameterOfBinaryTree(self, root):
    """
    :type root: TreeNode
    :rtype: int
    """
    self.ans = 0
    self.depth(root)
    return self.ans
    
def depth(self, root):
    if root is None:
        return 0
    left = self.depth(root.left)
    right = self.depth(root.right)
    self.ans = max(self.ans, left + right)
    return 1 + max(left, right)
```