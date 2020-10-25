---
title: Power of Three
tags: [LeetCode]
---

[326. Power of Three](https://leetcode.com/problems/power-of-three/)
#### Solution 1   
```python
def isPowerOfThree(self, n):
    """
    :type n: int
    :rtype: bool
    """
    if n <= 0:
        return False
    while n != 1:
        if n % 3 != 0:
            return False
        n /= 3
    return True
```
#### Solution 2 - Direct  
```python
def isPowerOfThree(self, n):
    """
    :type n: int
    :rtype: bool
    """
    # 1162261467 is 3^19,  3^20 is bigger than int  
    return  n>0 and 1162261467%n==0
```