---
title: Add Two Numbers
tags: [LeetCode]
---

[2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)
#### Solution
1. Need to keep `pre` and check if `pre` is 0 at the end.
[Cleaner Solution](https://leetcode.com/problems/add-two-numbers/discuss/1016/Clear-python-code-straight-forward)
```python
def addTwoNumbers(self, l1, l2):
    """
    :type l1: ListNode
    :type l2: ListNode
    :rtype: ListNode
    """
    res = ListNode(0)
    tail = res
    pre = 0
    while l1 or l2:
        val = pre
        if l1:
            val += l1.val
            l1 = l1.next
        if l2:
            val += l2.val
            l2 = l2.next
        tail.next = ListNode(val % 10)
        pre = val // 10
        tail = tail.next
    while pre != 0:
        tail.next = ListNode(pre % 10)
        pre = pre // 10
        tail = tail.next
    
    return res.next
```