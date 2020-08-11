---
title: Counting Bits
tags: [LeetCode]
---

[338. Counting Bits](https://leetcode.com/problems/counting-bits/)
#### Solution 
1. Use the characteristic of odd and even numbers, i.e. odd number end with 1 and even number end with 0.

1. Can use `num // 2` as memory.

[Solution](https://leetcode.com/problems/counting-bits/discuss/700961/Python3-DP-solution-by-utilizing-power-of-bitwise-operations)
```python
def countBits(self, num):
    """
    :type num: int
    :rtype: List[int]
    """
    bitCountList = [0]

    for i in range(1, num + 1):
        bitCountList.append(bitCountList[i >> 1] + (i & 1))

    return bitCountList
```