---
title: First Missing Positive
tags: [LeetCode]
---

[41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)
#### Solution  
1. The result should be maximally `n`, the length of `nums`.

1. We can use `nums` to keep track of which number is missing.

1. For any appeared `num`, set `nums[num]` to `None`.
    > Ignore all negative and num > `n`.
1. The first index of not `None` is the result.

```python
def firstMissingPositive(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    idx = 0
    l = len(nums)
    while idx < l:
        num = nums[idx]
        if num is None or num <= 0 or num > len(nums):
            idx += 1
            continue

        tmp = nums[num-1]
        nums[num-1] = None

        if idx != num-1 and tmp is not None:
            nums[idx] = tmp
        else:
            idx += 1

    for i, num in enumerate(nums):
        if num is None:
            continue
        else:
            return i + 1
            
    return l + 1
```
#### Solution - Improvement
[Solution](https://leetcode.com/problems/first-missing-positive/discuss/17080/Python-O(1)-space-O(n)-time-solution-with-explanation)
1. `nums[nums[i]%n]+=n`: It stores information about nums[i] and still keep the information of nums[nums[i]].
> `mod`: original value
> `div`: if the index has appeared

```python
def firstMissingPositive(self, nums):
    nums.append(0)
    n = len(nums)
    for i in range(len(nums)): #delete those useless elements
        if nums[i]<0 or nums[i]>=n:
            nums[i]=0
    for i in range(len(nums)): #use the index as the hash to record the frequency of each number
        nums[nums[i]%n]+=n
    for i in range(1,len(nums)):
        if nums[i]/n==0:
            return i
    return n
```