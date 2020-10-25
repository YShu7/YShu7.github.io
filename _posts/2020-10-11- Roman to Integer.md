---
title:  Roman to Integer
tags: [LeetCode]
---

[13.  Roman to Integer](https://leetcode.com/problems/roman-to-integer/)
#### Solution - Cleaner Solution  
[Solution](https://leetcode.com/problems/roman-to-integer/discuss/6537/My-Straightforward-Python-Solution)
1. If one letter is less than its latter one, this letter is subtracted.

1. Always remember to add the last letter.
```python
def romanToInt(self, s):
    roman = {'M': 1000,'D': 500 ,'C': 100,'L': 50,'X': 10,'V': 5,'I': 1}
    z = 0
    for i in range(0, len(s) - 1):
        if roman[s[i]] < roman[s[i+1]]:
            z -= roman[s[i]]
        else:
            z += roman[s[i]]
    return z + roman[s[-1]]
```