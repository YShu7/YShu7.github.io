---
title: Linked List Cycle II
tags: [LeetCode, Two Pointers]
---

[142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
#### Solution
1. H: distance from head to cycle entry E, D: distance from E to X, L: cycle length

1. 2H + 2D = H + D + L  -->  H + D = nL  --> H = nL - D

1.  Thus if two pointers start from head and X, respectively, one first reaches E, the other also reaches E.

[Reference](https://leetcode.com/problems/linked-list-cycle-ii/discuss/44783/Share-my-python-solution-with-detailed-explanation
> Can use `try` `except` to make the code cleaner.
```python
def detectCycle(self, head):
    """
    :type head: ListNode
    :rtype: ListNode
    """
    if head is None or head.next is None:
        return None
    
    slow, fast = head.next, head.next.next
        
    while slow != fast and slow is not None and fast is not None:
        slow = slow.next
        if fast.next is None:
            return None
        fast = fast.next.next
    
    if slow is None or fast is None:
        return None
    
    fast = head
    while slow != fast:
        slow, fast = slow.next, fast.next
        
    return fast
```