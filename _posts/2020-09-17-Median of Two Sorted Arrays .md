---
title: Median of Two Sorted Arrays
tags: [LeetCode]
---

[4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)
#### Solution - O(m+n)  
1. Use two pointers to track until visited `mid` numbers.

```python
def findMedianSortedArrays(self, nums1, nums2):
    """
    :type nums1: List[int]
    :type nums2: List[int]
    :rtype: float
    """
    i, j = 0, 0
    l1, l2 = len(nums1), len(nums2)
    mid = (l1 + l2 + 1) // 2
    is_odd = (l1 + l2) % 2 == 1
        
    res = 0
    for _ in range(mid):
        if i >= l1:
            res = nums2[j]
            j += 1
            continue
        if j >= l2:
            res = nums1[i]
            i += 1
            continue
            
        if nums1[i] < nums2[j]:
            res = nums1[i]
            i += 1
        else:
            res = nums2[j]
            j += 1
                
    if is_odd:
        return res
    
    if i >= l1:
        return float(res + nums2[j]) / 2
    if j >= l2:
        return float(res + nums1[i]) / 2
    return float(res + (nums1[i] if nums1[i] < nums2[j] else nums2[j])) / 2
```
#### Solution - O(log m+n)  
[Solution](https://leetcode.com/problems/median-of-two-sorted-arrays/discuss/2481/Share-my-O(log(min(mn)))-solution-with-explanation)
