---
title: Merge Two Sorted Lists
tags: LeetCode [Linked List]
---

[21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
#### Solution 
1. Keep 3 pointer, 1 for l1, 1 for l2 and 1 for the merged list.

1. When l1 or l2 has reached to the end, link the other one directly to the merged list, without copying the value.
```python
def mergeTwoLists(self, l1, l2):
    while l1 is not None and l2 is not None:
        if l1.val < l2.val:
            tmp = l1.next
            ptr.next = ListNode(l1.val)
            ptr = ptr.next
            l1 = tmp
        else:
            tmp = l2.next
            ptr.next = ListNode(l2.val)
            ptr = ptr.next
            l2 = tmp
        
    if l1 is None and l2 is not None:
        ptr.next = l2
            
    if l2 is None and l1 is not None:
        ptr.next = l1
                
    return start.next
```