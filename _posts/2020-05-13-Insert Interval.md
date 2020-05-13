---
title: Insert Interval
tags: LeetCode
---

[57. Insert Interval](https://leetcode.com/problems/insert-interval/)

#### Solution 1 - Traversal
1. Traverse the interval list one by one. Count the number of intervals that are considered.  
1. If the newInterval is **completely larger** than interval, insert the interval.  
1. If the newInterval is **completely smaller** than interval, break.  
1. If the newInterval is overlapping with the interval, merge it.  
1. If the considered intervals is smaller than the length of the intervals, it means there're still some we didn't append. 
Append them to the end of result. Else, append the updated newInterval only.  
Complexity: O(n)
```python
def insert(self, intervals, newInterval):
    """
    :type intervals: List[List[int]]
    :type newInterval: List[int]
    :rtype: List[List[int]]
    """
    newIntervals = []
    t = 0
    
    for interval in intervals:
        if interval[1] < newInterval[0]:
            newIntervals.append(interval)
        elif interval[0] > newInterval[1]:
            break;
        else:
            newInterval[0] = min(interval[0], newInterval[0])
            newInterval[1] = max(interval[1], newInterval[1])
        t += 1
    
    newIntervals.append(newInterval)
    return newIntervals + intervals[t:] if t<len(intervals) else newIntervals
```
#### Solution 2 - Keep pointer
1. Keep two pointer for where the newInterval intersect with the intervals.  
1. If the pointers point to the same interval, it means the newInterval doesn't need merging.  
1. Else merge newInterval with the intervals.  
Complexity: O(n)
```python
def insert(self, intervals, newInterval):
    """
    :type intervals: List[List[int]]
    :type newInterval: List[int]
    :rtype: List[List[int]]
    """
    l = len(intervals)
    i = 0
    
    # Until not completely smaller
    while(i < l and intervals[i][1] < newInterval[0]):
        i += 1

    j = i
    # Until completely larger
    while(j < l and intervals[j][0] <= newInterval[1]):
        j += 1


    if i == j:
        ret = newInterval
    else:
        start = min(intervals[i][0], newInterval[0])
        end = max(intervals[j-1][1],newInterval[1])
        ret = [start,end]

    return intervals[:i] + [ret] + intervals[j:]
```
#### Solution 3 - Sort then merge