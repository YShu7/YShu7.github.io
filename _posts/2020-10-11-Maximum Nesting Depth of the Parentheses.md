---
title: Maximum Nesting Depth of the Parentheses
tags: [LeetCode]
---

[1614. Maximum Nesting Depth of the Parentheses](https://leetcode.com/problems/maximum-nesting-depth-of-the-parentheses/)
#### Solution  
1. The string is a valid parentheses string. We only need to count`(` for the number of parentheses.

1. Remember to update the result if possible.

```python
def maxDepth(self, s):
    res = cur = 0
    for c in s:
        if c == '(':
            cur += 1
            res = max(res, cur)
        if c == ')':
            cur -= 1
    return res
```