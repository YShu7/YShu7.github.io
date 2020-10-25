---
title: Coordinate With Maximum Network Quality
tags: [LeetCode]
---

[1620. Coordinate With Maximum Network Quality](https://leetcode.com/problems/coordinate-with-maximum-network-quality/)
You are given an array of network towers towers and an integer radius, where towers[i] = [xi, yi, qi] denotes the ith network tower with location (xi, yi) and quality factor qi. All the coordinates are integral coordinates on the X-Y plane, and the distance between two coordinates is the Euclidean distance.

The integer radius denotes the maximum distance in which the tower is reachable. The tower is reachable if the distance is less than or equal to radius. Outside that distance, the signal becomes garbled, and the tower is not reachable.

The signal quality of the ith tower at a coordinate (x, y) is calculated with the formula ⌊qi / (1 + d)⌋, where d is the distance between the tower and the coordinate. The network quality at a coordinate is the sum of the signal qualities from all the reachable towers.

Return the integral coordinate where the network quality is maximum. If there are multiple coordinates with the same network quality, return the lexicographically minimum coordinate.

#### Solution  
1. Get the signal strength at each tower.
    > unreasonable assumption, should get the single strength at all possible points.
1. Select the one with the strongest signal.
```python
def bestCoordinate(self, towers, radius):
    """
    :type towers: List[List[int]]
    :type radius: int
    :rtype: List[int]
    """
    maxres, maxidx = 0, (0, 0)
    dic = [[0] * len(towers) for _ in range(len(towers))]
    for i, (x1, y1, q1) in enumerate(towers):
        res = 0
        for j, (x2, y2, q2) in enumerate(towers):
            if j <= i:
                d = dic[i][j]
            else:
                d = sqrt((x1 - x2) ** 2 + (y1 - y2) ** 2)
                dic[i][j] = d
                dic[j][i] = d
                
            if d > radius:
                continue
            res += math.floor(q2 / (1 + d))
        if res > maxres:
            maxres = res
            maxidx = (x1, y1)
        elif res == maxres:
            if x1 < maxidx[0] or (x1 == maxidx[0] and y1 < maxidx[1]):
                maxidx = (x1, y1)
    return maxidx
```