---
title: Decode Ways
tags: LeetCode, [Dynamic Programming]
---

[91. Decode Ways](https://leetcode.com/problems/decode-ways/)
#### Solution
1. The decode ways depend on how many ways the previous substring can be decoded.  
1. The numbers are from 1 to 26, it is possible that one digit can be in two kind of combination.  
1. We can keep track of `pre_one` and `pre_two`.
> Note: When the digit is 0, its `pre_one` need to be erased, because it cannot be decoded as a single number.  

------
e.g. 10423  
i = 0: `pre_two` = 0, `pre_one` = 0, `curr` = 1 -> [1]  
i = 1: `pre_two` = 0, `pre_one` = 1 (change to 0), `curr` = 1 -> [10]  
i = 2: `pre_two` = 0, `pre_one` = 1, `curr` = `pre_one` = 1 -> [10, 4]
i = 3: `pre_two` = 1, `pre_one` = 1, `curr` = `pre_one` = 1 -> [10, 4, 2]
i = 4: `pre_two` = 1, `pre_one` = 1, `curr` = `pre_one` + `pre_two` = 2 -> [10, 4, 2, 3], [10, 4, 23]
------

```python
def numDecodings(self, s):
    """
    :type s: str
    :rtype: int
    """
    pre_two, pre_one, res = 0, 0, 0
    for i, num in enumerate(s[0:]):
        pre_two, pre_one = pre_one, res
        if self.isDecodable(num):
            res = pre_one if i != 0 else 1
        else:
            res = 0
            pre_one = 0
        if self.isDecodable(s[i-1:i+1]):
            res += pre_two if i != 1 else 1
        if res == 0:
            return 0
    return res
        
def isDecodable(self, num):
    if num == '':
        return False
    num = int(num)
    if num > 0 and num < 27:
        return True
    return False
```