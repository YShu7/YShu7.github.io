---
title: Count Sorted Vowel Strings
tags: [LeetCode, Dynamic Programming]
---

[1641. Count Sorted Vowel Strings](https://leetcode.com/problems/count-sorted-vowel-strings/)
Given an integer n, return the number of strings of length n that consist only of vowels (a, e, i, o, u) and are lexicographically sorted.

A string s is lexicographically sorted if for all valid i, s[i] is the same as or comes before s[i+1] in the alphabet.

#### Solution  
```python
def countVowelStrings(self, n: int) -> int:
    @lru_cache(None)
    def dp(n, i):
        if n == 0: return 1  # Found a valid answer
        if i == 5:  return 0  # Reach to length of vowels [a, e, i, o, u]
        ans = dp(n, i + 1)  # Skip vowels[i]
        ans += dp(n - 1, i)  # Include vowels[i]
        return ans

    return dp(n, 0)
```