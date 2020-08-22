---
title: Merge k Sorted Lists
tags: LeetCode
---

[23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

#### Solutions - Divide and Conquer  
1. Use a dummy Node(0) to represent the first Node.

1. When one of the lists has reached to the end, can directly append another list to the end.

```python
def mergeKLists(self, lists):
    """
    :type lists: List[ListNode]
    :rtype: ListNode
    """
    l = len(lists)
    if l == 0:
        return None
                
    if l == 1:
        return lists[0]
    
    mid = l // 2
    left = self.mergeKLists(lists[:mid])
    right = self.mergeKLists(lists[mid:])
    return self.merge(left, right)

def merge(self, left, right):
    res = end = ListNode(0)
    while left and right:
        if left.val < right.val:
            end.next = left
            left = left.next
        else:
            end.next = right
            right = right.next
        end = end.next
    end.next = left or right
    return res.next
```