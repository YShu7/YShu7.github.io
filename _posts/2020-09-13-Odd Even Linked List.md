---
title: Odd Even Linked List
tags: [LeetCode, Linked List, Two Pointers]
---

[328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)
#### Solution  
1. Use two pointer to keep track of odd and even nodes.

1. Note to deal with `head.next.next` when `head.next` is None
```python
def oddEvenList(self, head):
    """
    :type head: ListNode
    :rtype: ListNode
    """
    dummy1 = odd = ListNode(0)
    dummy2 = even = ListNode(0)
    while head:
        odd.next = head
        even.next = head.next
        odd = odd.next
        even = even.next
        head = head.next.next if even else None
    odd.next = dummy2.next
    return dummy1.next
```