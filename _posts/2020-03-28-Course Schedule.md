---
title: Course Schedule
tags: LeetCode
---

[207. Course Schedule](https://leetcode.com/problems/course-schedule/)

#### Solutions
1. Use DFS to traverse through all courses.  
2. Mark a course as visited if all its pre-requisites are visited.  
3. Any course that was not visited returns `False`.  