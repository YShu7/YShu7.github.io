---
title: Count Unhappy Friends
tags: [LeetCode]
---

[1583. Count Unhappy Friends](https://leetcode.com/problems/count-unhappy-friends/)
#### Solution  
1. Get all friends `k` that are more preferred than `i`'s pair by `i`.

1. If `k` also prefers `i` than its pair, then they're unhappy friends.

```python
def unhappyFriends(self, n, preferences, pairs):
    """
    :type n: int
    :type preferences: List[List[int]]
    :type pairs: List[List[int]]
    :rtype: int
    """
    dic = {}
    for i, j in pairs:
        dic[i] = preferences[i][:preferences[i].index(j)]
        dic[j] = preferences[j][:preferences[j].index(i)]
       
    res = 0
    for i in dic:
        for j in dic[i]: # j is preferred than i's pair
            if i in dic[j]: # i is preferred than j's pair
                res += 1
                break # only one unhappy friend for each key
    return res
```