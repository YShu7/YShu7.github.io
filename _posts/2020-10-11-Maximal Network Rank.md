---
title: Maximal Network Rank
tags: [LeetCode]
---

[1615. Maximal Network Rank](https://leetcode.com/problems/maximal-network-rank/)

There is an infrastructure of n cities with some number of roads connecting these cities. Each roads[i] = [ai, bi] indicates that there is a bidirectional road between cities ai and bi.

The network rank of two different cities is defined as the total number of directly connected roads to either city. If a road is directly connected to both cities, it is only counted once.

The maximal network rank of the infrastructure is the maximum network rank of all pairs of different cities.

Given the integer n and the array roads, return the maximal network rank of the entire infrastructure.

#### Solution  
1. Sort the rank of networks. Keep any directly connected pairs.

1. Loop through all pairs.

```python
def maximalNetworkRank(self, n, roads):
    """
    :type n: int
    :type roads: List[List[int]]
    :rtype: int
    """
    ranks = [(0, i) for i in range(n)]
    dic = collections.defaultdict(set)
    for a, b in roads:
        ranks[a] = (ranks[a][0] + 1, ranks[a][1])
        ranks[b] = (ranks[b][0] + 1, ranks[b][1])
        dic[a].add(b)
        dic[b].add(a)
        
    ranks = sorted(ranks, reverse=True)
    
    res = 0
    for i in range(n):
        tmp = 0
        for j in range(i+1, n):
            c = 0
            l, r = ranks[i], ranks[j]
            if l[1] in dic[r[1]] or r[1] in dic[l[1]]: # if they're connected, -1
                c = l[0] + r[0] - 1
            else:
                c = l[0] + r[0]

            if c < tmp:
                break
            else:
                tmp = max(tmp, c)
        if tmp < res:
            break
        else:
            res = max(res, tmp)
    return res
```