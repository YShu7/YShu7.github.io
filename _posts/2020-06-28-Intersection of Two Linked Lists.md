---
title: Intersection of Two Linked Lists
tags: [LeetCode, Linked List]
---

[160.  Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)
#### Solution 1
1. The intersection must be the "tail" of both linked list.

1. Use two pointer to traverse the linked list from different head, we can achieve the route as 
`A + intersection + B + intersection` and `B + intersection + A + intersection`.

1. After `A/B + intersection + B/A`, the pointers are pointing to the same position. Return the pointer's position.

1. If the pointers never meet, there is no intersection between them.

```python
def getIntersectionNode(self, headA, headB):
    """
    :type head1, head1: ListNode
    :rtype: ListNode
    """
    pA, pB = headA, headB
    
    while pA != pB:
        if pA is None:
            pA = headB
        else:
            pA = pA.next
        if pB is None:
            pB = headA
        else:
            pB = pB.next
    return pA
```