---
title: Palindromic Substrings
tags: [LeetCode, Manacher's Algorithm]
---

[647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)

#### Solutions
**Solution 1:**

For each char in the string, there are 2 kinds of palindromic substring possible:

1. odd substring

2. even substring

Expand from each char as a center for both odd and even substring.

**Solution 2:** Manacher's Algorithm
[Solution](https://medium.com/hackernoon/manachers-algorithm-explained-longest-palindromic-substring-22cb27a5e96f#:~:text=Manacher's%20Algorithm%20Explained%E2%80%94%20Longest%20Palindromic%20Substring,-Mithra%20Talluri&text=Manacher's%20Algorithm%20helps%20us%20find,insights%20into%20how%20palindromes%20work.)
```python
def countSubstrings(self, s):
    """
    :type s: str
    :rtype: int
    """
    A = '@#' + '#'.join(s) + '#$'
    S = [0] * len(A)
    center = 0
    right = 0
    
    for i in range(len(A)-1):
        if i < right:
            S[i] = min(S[2*center - i], right-i)
        
        while A[i+S[i]+1] == A[i-S[i]-1]:
            S[i] += 1
        if i+S[i] > right:
            center = i
            right = i + S[i]
            
    return sum((v+1)/2 for v in S)
```