---
title: Arithmetic Subarrays
tags: [LeetCode]
---

[1630. Arithmetic Subarrays](https://leetcode.com/problems/arithmetic-subarrays/)
A sequence of numbers is called arithmetic if it consists of at least two elements, and the difference between every two consecutive elements is the same. More formally, a sequence s is arithmetic if and only if `s[i+1] - s[i] == s[1] - s[0]` for all valid i.
#### Solution - Brute Force  
1. Find and sort the subarray.

1. Check if it is arithmetic.
```python
def checkArithmeticSubarrays(self, nums, l, r):
    """
    :type nums: List[int]
    :type l: List[int]
    :type r: List[int]
    :rtype: List[bool]
    """
    res = []
    for i, j in zip(l, r):
        sub_nums = sorted(nums[i:j+1])
        print(sub_nums)
        if j - i == 1:
            res.append(True)
            continue
        dis = sub_nums[1] - sub_nums[0]
        sub_res = True
        for a, b in zip(sub_nums[:-1], sub_nums[1:]):
            if b - a != dis:
                sub_res = False
                break
        res.append(sub_res)
    return res
```
#### Solution - O(n) No sorting
[Solution](https://leetcode.com/problems/arithmetic-subarrays/discuss/909120/python3-using-set-no-sorting-O(MN))
1. Find the min and max of the subarray.

1. Find the step.

1. Check if all numbers following this step are in the subarray.
```python
def checkArithmeticSubarrays(self, nums: List[int], l: List[int], r: List[int]) -> List[bool]:
    def isArith(n):
        mx, mn, st = max(n), min(n), set(n)
        if (mx - mn)%(len(n)-1) != 0: return False
        step = (mx - mn)//(len(n)-1)
        if not step: return mx == mn
        for i in range(mn, mx, step):
            if i not in st:
                return False
        return True       
    res = []
    for i in range(len(l)):
        res.append(isArith(nums[l[i]:r[i]+1]))
    return res
```