---
title: Lexicographically Smallest String After Applying Operations
tags: [LeetCode]
---

[1625. Lexicographically Smallest String After Applying Operations](https://leetcode.com/problems/best-team-with-no-conflicts/)
You are given a string s of even length consisting of digits from 0 to 9, and two integers a and b.

You can apply either of the following two operations any number of times and in any order on s:

Add a to all odd indices of s (0-indexed). Digits post 9 are cycled back to 0. For example, if s = "3456" and a = 5, s becomes "3951".
Rotate s to the right by b positions. For example, if s = "3456" and b = 1, s becomes "6345".
Return the lexicographically smallest string you can obtain by applying the above operations any number of times on s.

A string a is lexicographically smaller than a string b (of the same length) if in the first position where a and b differ, string a has a letter that appears earlier in the alphabet than the corresponding letter in b. For example, "0158" is lexicographically smaller than "0190" because the first position they differ is at the third letter, and '5' comes before '9'.

#### Solution - BFS  
[Solution](https://leetcode.com/problems/lexicographically-smallest-string-after-applying-operations/discuss/899511/JavaPython-3-BFS-and-DFS-w-brief-explanation-and-analysis.)
```python
def findLexSmallestString(self, s: str, a: int, b: int) -> str:
    dq, seen, smallest = deque([s]), {s}, s
    while dq:
        cur = dq.popleft()
        if smallest > cur:
            smallest = cur
        addA = list(cur)    
        for i, c in enumerate(addA):
            if i % 2 == 1:
                addA[i] = str((int(c) + a) % 10)
        addA = ''.join(addA)        
        if addA not in seen:
            seen.add(addA);
            dq.append(addA)
        rotate = cur[-b :] + cur[: -b]
        if rotate not in seen:
            seen.add(rotate)
            dq.append(rotate)
    return smallest
```
#### Solution - DFS  
[Solution](https://leetcode.com/problems/lexicographically-smallest-string-after-applying-operations/discuss/899511/JavaPython-3-BFS-and-DFS-w-brief-explanation-and-analysis.)
```python
def findLexSmallestString(self, s: str, a: int, b: int) -> str: 
    def dfs(s: str) -> str:
        if s not in seen:
            seen.add(s)
            addA = list(s)
            for i, c in enumerate(addA):
                if i % 2 == 1:
                    addA[i] = str((int(c) + a) % 10)
            return min(s, dfs(''.join(addA)), dfs(s[b :] + s[: b]))
        return s    
            
    seen = set()
    return dfs(s)
```