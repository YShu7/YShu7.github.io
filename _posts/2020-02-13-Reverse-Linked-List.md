---
title: Reverse Linked List
tags: LeetCode
---

[206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

#### Solutions
1. **Solution 1:** Iterative
```python
def reverseList(self, head):
    """
    :type head: ListNode
    :rtype: ListNode
    """
    if head == None:
        return head
    next = head.next
    last = None
    while head!=None:
        tmp = head.next
        head.next = last
        last = head
        head = tmp
    
    return last
```
2. **Solution 2:** Recursive
```python
def reverseList(self, head):
    """
    :type head: ListNode
    :rtype: ListNode
    """
    def reverse(prev, curr):
        if curr == None:
            return prev
        else:
            tmp = curr.next
            curr.next = prev
            return reverse(curr, tmp)
        
    return reverse(None, head)
```
   