---
title: Flatten Binary Tree to Linked List
tags: [LeetCode]
---

[114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)
#### Solution  
1. Only when root has left subtree, we need to flatten it. 
When root doesn't have left subtree, update it to root.right.

1. The left subtree need to be apppended to root's right hand side.
 
1. The right subtree need to be appended to the end of left tree.

1. Flatten the left subtree and find its tail. Append right subtree to its tail.

1. Append the updated left subtree to root.right.
```python
def flatten(self, root):
    """
    :type root: TreeNode
    :rtype: None Do not return anything, modify root in-place instead.
    """
    while root:
        if root.left:
            self.flatten(root.left)
            tail = root.left
            while tail.right:
                tail = tail.right
            tail.right = root.right
            root.right, root.left = root.left, None
            root = tail.right
        else:
            root = root.right
```