---
title: Symmetric Tree
tags: LeetCode, [Tree], [DFS & BFS]
---

[101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)
#### Solution 
Check symmetrically
```python
def isSymmetric(self, root):
    """
    :type root: TreeNode
    :rtype: bool
    """
    def isSym(left, right):
        if left is None and right is None:
            return True
        if left and right and (left.val == right.val):
            return isSym(left.right, right.left) and isSym(left.left, right.right)
        return False
    
    return isSym(root, root)
```