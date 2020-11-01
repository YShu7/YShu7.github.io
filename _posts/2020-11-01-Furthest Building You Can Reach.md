---
title: Furthest Building You Can Reach
tags: [LeetCode]
---

[1642. Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)
You are given an integer array heights representing the heights of buildings, some bricks, and some ladders.

You start your journey from building 0 and move to the next building by possibly using bricks or ladders.

While moving from building i to building i+1 (0-indexed),

If the current building's height is greater than or equal to the next building's height, you do not need a ladder or bricks.  
If the current building's height is less than the next building's height, you can either use one ladder or (h[i+1] - h[i]) bricks.  
Return the furthest building index (0-indexed) you can reach if you use the given ladders and bricks optimally.  

#### Solution  
1. Use heap to store the gaps that needed least bricks.

1. When the size of heap > ladders, it means we must use bricks.

1. Pop out the smallest gap and reduct he number of bricks.

1. When there are not enough bricks, we stop.

```python
def furthestBuilding(self, A, bricks, ladders):
    heap = []
    for i in xrange(len(A) - 1):
        d = A[i + 1] - A[i]
        if d > 0:
            heapq.heappush(heap, d)
        if len(heap) > ladders:
            bricks -= heapq.heappop(heap)
        if bricks < 0:
            return i
    return len(A) - 1
```