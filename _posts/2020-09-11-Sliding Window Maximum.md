---
title: Sliding Window Maximum
tags: [LeetCode, Sliding Window, Dequeue]
---

[239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
#### Solution  
1. Use a queue to maintain the maximum at each position.

1. If a leftmost index in the queue is outside current window, pop it out.

1. If the end of the queue is smaller than `curr`, it will no longer be the maximum in the following sliding window, pop it out.

1. Push `curr` in.

1. The leftmost of the queue is the maximum for curr window. 
```python
def maxSlidingWindow(self, nums, k):
    """
    :type nums: List[int]
    :type k: int
    :rtype: List[int]
    """
    queue = collections.deque()
    res = []
    for i in range(len(nums)):
        while len(queue) != 0 and queue[0] < i - k + 1:
            queue.popleft()
        
        while len(queue) != 0 and nums[queue[-1]] < nums[i]:
            queue.pop()
        queue.append(i)
        
        if i >= k - 1:
            res.append(nums[queue[0]])
        
    return res
```
