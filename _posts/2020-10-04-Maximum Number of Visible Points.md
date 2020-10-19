---
title: Maximum Number of Visible Points
tags: [LeetCode]
---

[1610. Maximum Number of Visible Points](https://leetcode.com/problems/maximum-number-of-visible-points/)
You are given an array points, an integer angle, and your location, where location = [posx, posy] and points[i] = [xi, yi] both denote integral coordinates on the X-Y plane.

Initially, you are facing directly east from your position. You cannot move from your position, but you can rotate. In other words, posx and posy cannot be changed. Your field of view in degrees is represented by angle, determining how wide you can see from any given view direction. Let d be the amount in degrees that you rotate counterclockwise. Then, your field of view is the inclusive range of angles [d - angle/2, d + angle/2].

You can see some set of points if, for each point, the angle formed by the point, your position, and the immediate east direction from your position is in your field of view.

There can be multiple points at one coordinate. There may be points at your location, and you can always see these points regardless of your rotation. Points do not obstruct your vision to other points.

Return the maximum number of points you can see.

#### Solution  
1. Calculate the angles. Keep the points that are always seen.

1. Use sliding window to find the maximum number of points.
> Note that we need to go around the circle, so we duplicate the array and offset the second half by 2*pi.
>
```python
def visiblePoints(self, points, angle, location):
    """
    :type points: List[List[int]]
    :type angle: int
    :type location: List[int]
    :rtype: int
    """
    angles = []
    x, y = location
    always_seen = 0
    
    for px, py in points:
        if py == y and px == x:
            always_seen += 1
            continue
        angles.append(math.atan2(py - y, px - x))
    list.sort(angles)
    angles = angles + [x + 2 * math.pi for x in angles]
    angle = math.pi * angle / 180
    
    l = ans = 0
    for r in range(len(angles)):
        while angles[r] - angles[l] > angle:
            l += 1
        ans = max(ans, r - l + 1)
        
    return ans + always_seen
```