---
title: Trapping Rain Water
tags: [LeetCode]
---

[42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
#### Solution  
1. The 'height' of the water that one col can trap is **NOT** decied by the col height.
It is decided by the maximum height of neighbours to the left and right.

1. Use two array to store the maximum height of neighbours for each col from left and right.

1. The lower one of `start[i]` and `end[i]` decides the height of water, and the `height[i]` decides the occupied part.

```python
def trap(self, height):
    """
    :type height: List[int]
    :rtype: int
    """
    if len(height) == 0:
        return 0
    start, end = [height[0]], [height[-1]]
    res = 0
    for h in height[1:]:
        start.append(max(start[-1], h))
    for h in reversed(height[1:]):
        end.append(max(end[-1], h))
    end = list(reversed(end))
    
    for i, h in enumerate(height):
        res += max(0, min(start[i], end[i]) - height[i])
    return res
```