---
title: Queue Reconstruction by Height
tags: [LeetCode]
---

[406. Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/)
#### Solution 
1. Consider the shortest person, his `k` is the index he should be at.

1. For the other people, use the same idea. Keep `None` to represent 'empty'.

```python
def reconstructQueue(self, people):
    """
    :type people: List[List[int]]
    :rtype: List[List[int]]
    """
    people.sort()
    empty = [None, None]
    res = [empty] * len(people)
    for p in people:
        space = 0
        i = 0
        while res[i] != empty or space != p[1]:
            r = res[i]
            if r == empty or r[0] >= p[0]:
                space += 1
            i += 1
        res[i] = p
    return res
```