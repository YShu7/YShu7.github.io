---
title: Partition Labels
tags: [LeetCode]
---

[763. Partition Labels](https://leetcode.com/problems/partition-labels/)
#### Solution
1. For each character, store its starting point and end point.

1. Loop through the string. Update the ending point whenever an encountered character ends later.

1. When the visited character goes beyond the `end`, a new substring can start. We store the previous substring's length.

```python
def partitionLabels(self, S):
    """
    :type S: str
    :rtype: List[int]
    """
    dic = {}
    res = []
    
    for i, c in enumerate(S):
        if c in dic:
            dic[c] = (dic[c][0], i)
        else:
            dic[c] = (i, i)
    start, end = -1, -1
    #print(dic)
    for i, c in enumerate(S):
        l, r = dic[c]
        if i > end:
            res.append(end - start + 1)
            start = i
            
        end = max(end, r)
        #print(i, c, start, end)
    res.append(end - start + 1)
    return res[1:]
```  