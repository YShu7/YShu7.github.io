---
title: Longest Valid Parentheses
tags: [LeetCode, Dynamic Programming]
---

[32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)
#### Solution - Stack  
[Solution](https://leetcode.com/problems/longest-valid-parentheses/discuss/14126/My-O(n)-solution-using-a-stack)
1. Use stack to keep track of all indexes.

1. When a valid parentheses pair is found, pop the indexes out. Otherwise push.

1. Thus the substring between each index in the stack is valid.

1. Get the longest one.

```python
def longestValidParentheses(self, s):
    """
    :type s: str
    :rtype: int
    """
    stack = []
    
    res = 0
    for i, c in enumerate(s):
        if c == '(':
            stack.append(i)
        if c == ')':
            if stack and s[stack[-1]] == '(':
                stack.pop()
            else:
                stack.append(i)
                    
    if not stack:
        return len(s)
    
    a = len(s)
    b = 0
    while stack:
        b = stack.pop()
        res = max(res, a-b-1)
        a = b
    res = max(res, a)
    return res
```
#### Solution - DP  
[Solution](https://leetcode.com/problems/longest-valid-parentheses/discuss/14133/My-DP-O(n)-solution-without-using-stack)
1. If `s[i]` is '(', set `longest[i]` to 0, because any string end with '(' cannot be a valid one.

1. Else if `s[i]` is ')'

     If `s[i-1]` is '(', `longest[i] = longest[i-2] + 2`

     Else if `s[i-1]` is ')' and `s[i-longest[i-1]-1] == '('`, `longest[i] = longest[i-1] + 2 + longest[i-longest[i-1]-2]`
    > Check if the character before the previous valid substring can match with ')'
    > If can, add the previous **2** valid substring together with `+2`, since the substring before the previous valid substring CAN be connected with this additional valid pair.  