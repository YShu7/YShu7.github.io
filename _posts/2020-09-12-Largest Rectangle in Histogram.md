---
title: Largest Rectangle in Histogram
tags: [LeetCode, Stack, Dynamic Programming]
---

[84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
#### Solution - Memorization  
[Solution](https://leetcode.com/problems/largest-rectangle-in-histogram/discuss/28902/5ms-O(n)-Java-solution-explained-(beats-96))

1. Find the leftmost and rightmost index for each histogram.

1. Use a memory array to 'jump' to the previous satisfied neighbour instead of looping through all neighbours.

```python
def largestRectangleArea(self, heights):
    """
    :type heights: List[int]
    :rtype: int
    """
    res = 0
    l = len(heights)
    # Jump for searching
    min_h_left, min_h_right = [], []
    for i in range(l):
        end_h = heights[i]
        j = i-1
        while j >= 0 and end_h <= heights[j]:
            j = min_h_left[j]
        min_h_left.append(j)
            
    for i in range(l-1, -1, -1):
        end_h = heights[i]
        j = i+1
        while j < l and end_h <= heights[j]:
            j = min_h_right[l-1-j]
        min_h_right.append(j)
    min_h_right = list(reversed(min_h_right))
    
    for i, start_h in enumerate(heights):
        res = max(res, start_h, (min_h_right[i] - min_h_left[i] - 1) * start_h)
        
    return res
```
#### Solution - Stack
[Solution](https://leetcode.com/problems/largest-rectangle-in-histogram/discuss/28917/AC-Python-clean-solution-using-stack-76ms)
1. Keep a stack maintaining the indexes of buildings with ascending height

1. For each rectangle with the heights of the popped histogram, 
its rightmost neighbour is `i`, its leftmost neighbor is the next item in the stack.
    > Because both of them are the immediate histogram with height < current height.
1. To deal with the boundary (at index 0), a dummy height 0 and index -1 is provided.

```python
def largestRectangleArea(self, heights):
    """
    :type heights: List[int]
    :rtype: int
    """
    res = 0
    l = len(heights)
    if l == 0:
        return 0
    
    heights.append(0)
    stack = [-1]
    for i, h in enumerate(heights):
        while len(stack) != 0 and heights[stack[-1]] > h:
            index = stack.pop()
            res = max(res, (i - stack[-1] - 1) * heights[index])
        stack.append(i)
    return res
```