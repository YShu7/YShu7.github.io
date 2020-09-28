---
title: Reverse String
tags: [LeetCode]
---

[344. Reverse String](https://leetcode.com/problems/reverse-string/)
#### Solution  
1. Reverse from both start and end.
```python
def reverseString(self, s):
    """
    :type s: List[str]
    :rtype: None Do not return anything, modify s in-place instead.
    """
    l = len(s)
    for i in range(l // 2):
        lc, rc = s[i], s[l-1-i]
        tmp = lc
        s[i] = rc
        s[l-1-i] = tmp
```