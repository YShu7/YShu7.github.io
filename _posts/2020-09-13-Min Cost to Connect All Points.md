---
title: Min Cost to Connect All Points
tags: [LeetCode, MST, Prim's Algorithm]
---

[1584. Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

You are given an array points representing integer coordinates of some points on a 2D-plane, where points[i] = [xi, yi].

The cost of connecting two points [xi, yi] and [xj, yj] is the manhattan distance between them: |xi - xj| + |yi - yj|, where |val| denotes the absolute value of val.

Return the minimum cost to make all points connected. All points are connected if there is exactly one simple path between any two points.

#### Solution - Prim's Algo
1. Initialize a tree with a single vertex, chosen arbitrarily from the graph.

1. Grow the tree by one edge: of the edges that connect the tree to vertices not yet in the tree, find the minimum-weight edge, and transfer it to the tree.

1. Repeat step 2 (until all vertices are in the tree).

```python
def minCostConnectPoints(self, points):
    """
    :type points: List[List[int]]
    :rtype: int
    """
    # Prim
    dic = {}
    l = len(points)
    for i, pi in enumerate(points):
        dic[i] = []
        for j, pj in enumerate(points):
            dis = self.getDistance(pi, pj)
            dic[i].append((dis, j))

    heap = dic[0]
    heapq.heapify(heap)

    visited, visited_num = [False] * len(points), 0
    
    visited[0] = True
    visited_num += 1
    
    res = 0
    while heap:
        min_dis, added_node = heapq.heappop(heap)
        if visited[added_node]:
            continue
        visited[added_node] = True
        visited_num += 1
        res += min_dis

        if visited_num >= l:
            break
        for neighbour in dic[added_node]:
            heapq.heappush(heap, neighbour)
            
    return res

def getDistance(self, a, b):
    return abs(a[0] - b[0]) + abs(a[1] - b[1])
```