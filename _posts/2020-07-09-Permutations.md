---
title: Permutations
tags: [LeetCode]
---

[46. Permutations](https://leetcode.com/problems/permutations/)
#### Solution 
dfs(nums = [1, 2, 3] , path = [] , result = [] )  
|____ dfs(nums = [2, 3] , path = [1] , result = [] )  
|      |___dfs(nums = [3] , path = [1, 2] , result = [] )  
|      |    |___dfs(nums = [] , path = [1, 2, 3] , result = [[1, 2, 3]] )  
|      |___dfs(nums = [2] , path = [1, 3] , result = [[1, 2, 3]] )  
|           |___dfs(nums = [] , path = [1, 3, 2] , result = [[1, 2, 3], [1, 3, 2]] )  
|____ dfs(nums = [1, 3] , path = [2] , result = [[1, 2, 3], [1, 3, 2]] )  
|      |___dfs(nums = [3] , path = [2, 1] , result = [[1, 2, 3], [1, 3, 2]] )  
|      |    |___dfs(nums = [] , path = [2, 1, 3] , result = [[1, 2, 3], [1, 3, 2], [2, 1, 3]] )  
|      |___dfs(nums = [1] , path = [2, 3] , result = [[1, 2, 3], [1, 3, 2], [2, 1, 3]] )  
|           |___dfs(nums = [] , path = [2, 3, 1] , result = [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1]] )  
|____ dfs(nums = [1, 2] , path = [3] , result = [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1]] )  
       |___dfs(nums = [2] , path = [3, 1] , result = [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1]] )  
       |    |___dfs(nums = [] , path = [3, 1, 2] , result = [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2]] )  
       |___dfs(nums = [1] , path = [3, 2] , result = [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2]] )  
            |___dfs(nums = [] , path = [3, 2, 1] , result = [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]] )  
```python
def permute(self, nums):
    """
    :type nums: List[int]
    :rtype: List[List[int]]
    """
    L = len(nums)
    res = []
    def dfs(l, remaining, path):
        if l == L:
            res.append(path)
        for i, r in enumerate(remaining):
            dfs(l+1, remaining[:i] + remaining[i+1:], path + [r])
    dfs(0, nums, [])
    return res
```