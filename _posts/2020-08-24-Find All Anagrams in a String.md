---
title: Find All Anagrams in a String
tags: [LeetCode]
---

[438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
#### Solution  
1. Use a sliding window to scan through `s`.

1. Check if the characters in the window is the same as those in `p`.
> Can use an array where index represents the character and element represents the count as well.
```python
def findAnagrams(self, s, p):
    """
    :type s: str
    :type p: str
    :rtype: List[int]
    """
    pdic = collections.Counter(p)
    l = len(p)

    if len(s) < l:
        return []
        
    sdic = collections.Counter(s[:l])
    
    for c in s:
        if c not in pdic:
            pdic[c] = 0
        if c not in sdic:
            sdic[c] = 0
     
    res = [0] if sdic == pdic else []
    for i in range(1, len(s) - l + 1):
        sdic[s[i-1]] -= 1
        sdic[s[i-1+l]] += 1
        
        if sdic == pdic:
            res.append(i)
        
    return res
```