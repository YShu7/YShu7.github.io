---
title: Copy List with Random Pointer
tags: [LeetCode, Deep Copy]
---

[138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)
#### Solution 1 - O(n) space
1. Keep a 2-way dictionary for `.random`
```python
def copyRandomList(self, head):
    """
    :type head: Node
    :rtype: Node
    """
    tail, htail = Node(0), head
    res = tail
    idx_node, node_idx = {}, {}
    idx = 0
    while htail:
        idx_node[idx] = Node(htail.val)
        node_idx[htail] = idx
        tail.next = idx_node[idx]
        tail = tail.next
        htail = htail.next
        idx += 1

    tail, htail = res, head
    while htail:
        tail = tail.next
        if htail.random:
            tail.random = idx_node[node_idx[htail.random]]
        htail = htail.next
    return res.next
```
#### Solution 2 - O(1) space
[Solution](https://www.cnblogs.com/zuoyuan/p/3745126.html)
```python
def copyRandomList1(self, head):
    if not head:
        return 
    # copy nodes
    cur = head
    while cur:
        nxt = cur.next
        cur.next = RandomListNode(cur.label)
        cur.next.next = nxt
        cur = nxt
    # copy random pointers
    cur = head
    while cur:
        if cur.random:
            cur.next.random = cur.random.next
        cur = cur.next.next
    # separate two parts
    second = cur = head.next
    while cur.next:
        head.next = cur.next
        head = head.next
        cur.next = head.next
        cur = cur.next
    head.next = None
    return second
```