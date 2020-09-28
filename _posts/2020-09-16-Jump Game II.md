---
title: Jump Game II
tags: [LeetCode, Greedy]
---

[45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)
#### Solution  
1. At each step, we can jump at a range `[l, r]`.

1. `l` is where we end up with at the previous step.
> All positions before it was checked already.

1. Keep the right most position `r` that one can jump within `times` jump.

1. When `r == len(nums) - 1`, we have reached our goal.

```python
def jump(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    if len(nums) <= 1: return 0
    l, r = 0, nums[0]
    times = 1
    while r < len(nums) - 1:
        times += 1
        nxt = max(i + nums[i] for i in range(l, r + 1))
        l, r = r, nxt
    return times
```