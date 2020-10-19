---
title: Even Odd Tree
tags: [LeetCode]
---

[1609. Even Odd Tree](https://leetcode.com/problems/even-odd-tree/)
A binary tree is named Even-Odd if it meets the following conditions:

The root of the binary tree is at level index 0, its children are at level index 1, their children are at level index 2, etc.  
For every even-indexed level, all nodes at the level have odd integer values in strictly increasing order (from left to right).  
For every odd-indexed level, all nodes at the level have even integer values in strictly decreasing order (from left to right).  
Given the root of a binary tree, return true if the binary tree is Even-Odd, otherwise return false.  

#### Solution - BFS 
1. Check each level independently.

```python
def isEvenOddTree(self, root):
    """
    :type root: TreeNode
    :rtype: bool
    """
    queue = [(root, 0)]
    while queue:
        lvl = queue[0][1]
        pre = 0
        while queue and queue[0][1] == lvl:
            curr = queue.pop(0)[0]
            if not curr:
                continue
            if lvl % 2 == 0:
                if curr.val % 2 == 0:
                    return False
                if pre != 0 and pre >= curr.val:
                    return False
            if lvl % 2 == 1:
                if curr.val % 2 == 1:
                    return False
                if pre != 0 and pre <= curr.val:
                    return False
            # print(curr.val, lvl)
            pre = curr.val    
            queue.append((curr.left, lvl+1))
            queue.append((curr.right, lvl + 1))
    return True
```