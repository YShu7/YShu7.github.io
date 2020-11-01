---
title: Count Substrings That Differ by One Character
tags: [LeetCode]
---

[1638. Count Substrings That Differ by One Character](https://leetcode.com/problems/count-substrings-that-differ-by-one-character/)
Given two strings s and t, find the number of ways you can choose a non-empty substring of s and replace a single character by a different character such that the resulting substring is a substring of t. In other words, find the number of substrings in s that differ from some substring in t by exactly one character.

For example, the underlined substrings in "computer" and "computation" only differ by the 'e'/'a', so this is a valid way.

Return the number of substrings that satisfy the condition above.

A substring is a contiguous sequence of characters within a string.

#### Solution - Compare and Expand   
1. Generate all substrings of `s` and `t` and the number of them.

1. Check if `substring_S` and `substring_T` only differ in one character.

```python
def countSubstrings(self, s, t):
    """
    :type s: str
    :type t: str
    :rtype: int
    """
    S, T = len(s), len(t)
    substring_S = collections.defaultdict(lambda: collections.Counter())
    substring_T = collections.defaultdict(lambda: collections.Counter())
    for l in range(1, S+1):
        for j in range(S-l+1):
            substring_S[l][s[j:j+l]] += 1
    for l in range(1, S+1):
        for j in range(T-l+1):
            substring_T[l][t[j:j+l]] += 1
    # print(substring_S, substring_T)
    
    res = 0
    for l in range(1, S+1):
        for kS, vS in substring_S[l].items():
            for kT, vT in substring_T[l].items():
                diff = [0 if a == b else 1 for a, b in zip(kS, kT)]
                if sum(diff) == 1:
                    res += vS * vT
    return res
```
#### Solution - Process Missmatches  
[Solution](https://leetcode.com/problems/count-substrings-that-differ-by-one-character/discuss/917701/C%2B%2BJavaPython3-O(n-3)-greater-O(n-2))
1. Find the mismatched character.

1. Count matching characters to the left and to the right.

```python
def countSubstrings(self, s, t):
    res = 0
    for i, cs in enumerate(s):
        for j, ct in enumerate(t):
            if cs != ct:
                l, r = 1, 1
                while min(i-l, j-l) >= 0 and s[i-l] == t[j-l]:
                    l += 1
                while i+r < len(s) and j+r < len(t) and s[i+r] == t[j+r]:
                    r += 1
                res += l * r
    return res
```