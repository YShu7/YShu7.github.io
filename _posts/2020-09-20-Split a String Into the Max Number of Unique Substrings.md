---
title: Split a String Into the Max Number of Unique Substrings
tags: [LeetCode, Backtracking]
---

[1593. Split a String Into the Max Number of Unique Substrings](https://leetcode.com/problems/split-a-string-into-the-max-number-of-unique-substrings/)

Given a string s, return the maximum number of unique substrings that the given string can be split into.

You can split string s into any list of non-empty substrings, where the concatenation of the substrings forms the original string. However, you must split the substrings such that all of them are unique.

A substring is a contiguous sequence of characters within a string.

#### Solution  
1. Try to divide the first `i` characters as a substring.

1. If the substring has not been visited, we can continue to divide and compare this result with the current best one.

1. If the substring has been visited, go to the next `i`.

```python
def maxUniqueSplit(self, s):
    """
    :type s: str
    :rtype: int
    """
    w_set = set()
    return self.helper(s, w_set)
    
def helper(self, s, w_set):
    res = 0
    for i in range(1,len(s)+1):
        pre = s[:i]
        if pre not in w_set:
            w_set.add(pre)
            res = max(res, 1+self.helper(s[i:], w_set))
            w_set.remove(pre)
            # check if we use a substring which starts from each index, and end all to way to the end of the string
            # how many substrings are possible
    return res
```