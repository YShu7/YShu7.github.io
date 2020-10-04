---
title: Delete Node in a Linked List
tags: [LeetCode]
---

[237. Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)
#### Solution  
1. Copy the next node's val to next.next node

1. Move the pointer

```python
def deleteNode(self, node):
    """
    :type node: ListNode
    :rtype: void Do not return anything, modify node in-place instead.
    """
    node.val = node.next.val
    node.next = node.next.next
```