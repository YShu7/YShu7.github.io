---
title: Count Unhappy Friends
tags: [LeetCode]
---

[1583. Count Unhappy Friends](https://leetcode.com/problems/count-unhappy-friends/)

You are given a list of preferences for n friends, where n is always even.

For each person i, preferences[i] contains a list of friends sorted in the order of preference. In other words, a friend earlier in the list is more preferred than a friend later in the list. Friends in each list are denoted by integers from 0 to n-1.

All the friends are divided into pairs. The pairings are given in a list pairs, where pairs[i] = [xi, yi] denotes xi is paired with yi and yi is paired with xi.

However, this pairing may cause some of the friends to be unhappy. A friend x is unhappy if x is paired with y and there exists a friend u who is paired with v but:

x prefers u over y, and
u prefers x over v.
Return the number of unhappy friends.

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