---
title: Palindrome Linked List
tags: [LeetCode, Linked List, Two Pointers, *]
---

[234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)
#### Solution 1
1. Get the mid of the linked list.

1. Reverse the first half of the linked list.

1. Traverse from the head and the node immediate after mid. Check if they are the same.

Time Complexity: O(n)
Space Complexity: O(1)

```python
def isPalindrome(self, head):
    """
    :type head: ListNode
    :rtype: bool
    """
    curr = head
    l = 0
    while curr:
        curr = curr.next
        l += 1
    
    idx = 0
    prev, curr = None, head
    while idx < l / 2:
        tmp = curr.next
        curr.next = prev
        prev = curr
        curr = tmp
        idx += 1

    if l % 2 != 0:
        curr = curr.next
    while idx > 0:
        if curr.val != prev.val:
            return False
        curr = curr.next
        prev = prev.next
        idx -= 1
    return True
```

#### Solution 1a
Use a fast pointer with `step = 2` to find the mid point.
```python
def isPalindrome(self, head):
    rev = None
    slow = fast = head
    while fast and fast.next:
        fast = fast.next.next
        rev, rev.next, slow = slow, rev, slow.next
    if fast:
        slow = slow.next
    while rev and rev.val == slow.val:
        slow = slow.next
        rev = rev.next
    return not rev
```
