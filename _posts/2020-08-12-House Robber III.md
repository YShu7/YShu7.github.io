---
title: House Robber III
tags: [LeetCode, DFS]
---

[337. House Robber III](https://leetcode.com/problems/house-robber-iii/)
#### Solution  
1. A house can be stolen or not stolen.

1. Use a tuple to store money that the thief can rob when the house is steal and not.

1. If a house is stolen, its neighbours **CANNOT** be stolen. Its neighbours money can be decided.

1. If a house is not stolen, its neighbours **CAN** be stolen. We choose max from its neighbours money.

```python
def rob(self, root):
    """
    :type root: TreeNode
    :rtype: int
    """
    return max(self.helper(root))
def helper(self, root):
    if not root:
        return (0, 0)
    left = self.helper(root.left)
    right = self.helper(root.right)
    return (root.val + left[1] + right[1], 
            max(left) + max(right))
```