---
title: Sum of All Odd Length Subarrays
tags: [LeetCode]
---

[1588. Sum of All Odd Length Subarrays](https://leetcode.com/problems/sum-of-all-odd-length-subarrays/)
Given an array of positive integers arr, calculate the sum of all possible odd-length subarrays.

A subarray is a contiguous subsequence of the array.

Return the sum of all odd-length subarrays of arr.

#### Solution 1 - Brute Force  
```python
def sumOddLengthSubarrays(self, arr):
    """
    :type arr: List[int]
    :rtype: int
    """
    res = sum(arr)
    
    l = len(arr)
    sub_l = 3
    while sub_l <= l:
        for i in range(l-sub_l+1):
            res += sum(arr[i:i+sub_l])
        sub_l += 2
    return res
```
#### Solution 2 - Consider the contribution of A[i]  
[Solution](https://leetcode.com/problems/sum-of-all-odd-length-subarrays/discuss/854184/JavaC%2B%2BPython-O(N)-Time-O(1)-Space)
1. Consider the subarray that contains `A[i]`

1. We can take 0,1,2..,i elements on the left, from `A[0]` to `A[i]`, we have i + 1 choices.

1. we can take 0,1,2..,n-1-i elements on the right, from `A[i]` to `A[n-1]`, we have n - i choices.

1. In total, there are (i + 1) * (n - i) subarrays, that contains `A[i]`. And there are ((i + 1) * (n - i) + 1) / 2 subarrays with odd length, that contains `A[i]`.

```python
def sumOddLengthSubarrays(self, A):
    res, n = 0, len(A)
    for i, a in enumerate(A):
        res += ((i + 1) * (n - i) + 1) / 2 * a
    return res
```