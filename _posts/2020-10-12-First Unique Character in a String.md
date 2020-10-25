---
title: First Unique Character in a String
tags: [LeetCode]
---

[387. First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)
#### Solution  
1. Get all indexes of characters in the string.
    > Can use array and ascii number instead.

1. If the length of indexes is 1, collect the index.

1. Find the minimum of them.
```python
def firstUniqChar(self, s):
    """
    :type s: str
    :rtype: int
    """
    dic = collections.defaultdict(list)
    for i, c in enumerate(s):
        dic[c].append(i)
    
    values = []
    for l in dic.values():
        if len(l) == 1:
            values += l
            
    return min(values) if values else -1
```
#### Solution - Faster  
> It's faster because s.index is a C function that python is calling. 
> So the python loop is changed to be the 26 characters, and the C loop is doing the heavy lifting searching the string. 
> From an algo perspective this is slower than the others. But good to know for python users for runtime speedup.
```python
def firstUniqChar(self, s):
    """
    :type s: str
    :rtype: int
    """
    
    letters='abcdefghijklmnopqrstuvwxyz'
    index=[s.index(l) for l in letters if s.count(l) == 1]
    return min(index) if len(index) > 0 else -1
```