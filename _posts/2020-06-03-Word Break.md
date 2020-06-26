---
title: Word Break
tags: [LeetCode, Dynamic Programming]
---

[139. Word Break](https://leetcode.com/problems/word-break/)
#### Solution 
1. Check if the word can be broke up to an index `i`.  
1. For any substring end at `i`, we check if there exists any breakable word end at `j` can be combined with a word in 
`wordDict` to form it.
> `j < i` because of DP.  
> `j > i - max_len` because the prefix word needs to be long enough.  
```python
def wordBreak(self, s, wordDict):
    """
    :type s: str
    :type wordDict: List[str]
    :rtype: bool
    """
    breakable = [True]
    max_len = max(map(len,wordDict+['']))

    for i in range(1, len(s)+1):
        breakable += any(breakable[j] and s[j:i] in wordDict for j in range(max(0, i-max_len),i)),
    return breakable[-1]
```