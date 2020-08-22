---
title: Letter Combinations of a Phone Number
tags: [LeetCode]
---

[17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
#### Solution  
1. Update results from the string up to `i-1`.
```python
dic = {
        '2': ['a', 'b', 'c'],
        '3': ['d', 'e', 'f'],
        '4': ['g', 'h', 'i'],
        '5': ['j', 'k', 'l'],
        '6': ['m', 'n', 'o'],
        '7': ['p', 'q', 'r', 's'],
        '8': ['t', 'u', 'v'],
        '9': ['w', 'x', 'y', 'z']
    }
def letterCombinations(self, digits):
    """
    :type digits: str
    :rtype: List[str]
    """
    if len(digits) == 0:
        return []
    substrings = self.letterCombinations(digits[1:])

    if substrings == []:
        substrings = ['']
    return [c + i for c in self.dic[digits[0]] for i in substrings]
```