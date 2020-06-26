---
title: Single Number
tags: LeetCode
---

[136. Single Number](https://leetcode.com/problems/single-number/)
#### Solution 1 
1. Use set to track all **seen** appeared numbers.

1. Remove **seen** number if it is re-visited.

1. The last one is the single number.

Space Complexity: O(n)

#### Solution 2 - Bitwise XOR
1. `0 ^ N = N` and `N ^ N = 0`

1. So XOR all number together, only the single number's value is preserved.

```python
def singleNumber(self, nums):
    for i in range(1,len(nums)):
        nums[0] ^= nums[i]
    return nums[0]
```

