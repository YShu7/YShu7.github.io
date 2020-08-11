---
title: Generate Parentheses
tags: [LeetCode]
---

[22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)
#### Solution 
1. As long as `open` > `close` in the sequence, the sequence is valid.

1. In this case we can either append '(' if we still have `open` quota, or append ')' if we have more `open` than 
`close` in the sequence.

1. When `close` is used up, we have generated all parentheses.

```python
def generateParenthesis(self, n):
    """
    :type n: int
    :rtype: List[str]
    """
    self.res = []

    def generate(op, cl, s):
        if op > 0:
            generate(op-1, cl, s+'(')
        if cl > op:
            generate(op, cl-1, s+')')
        if cl == 0:
            self.res.append(s)
        return self.res
    
    return generate(n, n, '')
```