---
title: Decode String
tags: [LeetCode]
---

[394. Decode String](https://leetcode.com/problems/decode-string/)
#### Solution  
1. Use a dictionary to store the decoded string.

1. When replaceing, we should go from outside to inside.
 
```python
def decodeString(self, s):
    """
    :type s: str
    :rtype: str
    """
    clauses = self.get_clauses_idx(s)
    dic = []
    for i in range(len(clauses)):
        left, right = clauses[i]
        
        k, k_start_idx = '', -1
        for j in range(left-1, -1, -1):
            if s[j] >= '0' and s[j] <= '9':
                k = s[j] + k
                k_start_idx = j
            else:
                break
        k = int(k)
        substring = s[left+1:right]
        dic.append((s[k_start_idx:right+1], substring * k))
    
    for i in range(len(dic)-1, -1, -1):
        s = s.replace(dic[i][0], dic[i][1])
    return s

def get_clauses_idx(self, s):
    stack, clauses = [], []
    for i, char in enumerate(s):
        if char == '[':
            stack.append((char, i))
        if char == ']':
            if stack[-1][0] == '[':
                top = stack.pop()
                clauses.append((top[1], i))
    return clauses
```
#### Solution - DFS
1. Can also use DFS to replace directly from inside to outside.

1. In this case the decoded string is not retrieved until all its inner string is decoded.

```python
def decodeString(self, s):
    if not s or len(s) == 0:
        return s
    result, position = self.dfs(0,s,0,'')
    return result

def dfs(self, position, s, prev_num, prev_str):
    while position < len(s):
        while s[position].isdigit():
            prev_num  = prev_num*10 + int(s[position])
            position += 1

        if s[position] == "[":
            #reset the prev_str
            returned_str, ending_pos = self.dfs(position+1, s, prev_num=0, prev_str="")
            #backtrack
            prev_str = prev_str + returned_str*prev_num
            position = ending_pos
            prev_num = 0
        #return the result
        elif s[position] == ']':
            return prev_str, position
        else:
            prev_str += s[position]
        position += 1
    return prev_str, position
```