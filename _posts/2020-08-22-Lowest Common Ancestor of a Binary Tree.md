---
title: Lowest Common Ancestor of a Binary Tree
tags: [LeetCode]
---

[236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
#### Solution  
1. If the current (sub)tree contains both p and q, then the function result is their LCA. 

1. If only one of them is in that subtree, then the result is that one of them. 

1. If neither are in that subtree, the result is null/None/nil.

```python
def lowestCommonAncestor(self, root, p, q):
    """
    :type root: TreeNode
    :type p: TreeNode
    :type q: TreeNode
    :rtype: TreeNode
    """
    if root is None:
        return root
    
    if root.val == p.val or root.val == q.val:
        return root
    left = self.lowestCommonAncestor(root.left, p, q)
    right = self.lowestCommonAncestor(root.right, p, q)
    
    if left is not None and right is not None:
        return root
    
    return left if left is not None else right
```