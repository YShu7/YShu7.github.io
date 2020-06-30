---
title: Path Sum III
tags: [LeetCode, Tree]
mathjax: true
---

[437. Path Sum III](https://leetcode.com/problems/path-sum-iii/)

[Solution](https://leetcode.com/problems/path-sum-iii/discuss/141424/Python-step-by-step-walk-through.-Easy-to-understand.-Two-solutions-comparison.-%3A-)

#### Solution 1
1. Consider the node need to achieve a target instead of the path to achieve a target.

1. For each node, consider all paths **start with** it, check if the path can achieve the target.

Complexity: $O(n^2)$

#### Solution 2
1. Memorize all values of paths from **root** to current node. Store the values as `{value: frequency}`.

1. `currPathSum - oldPathSum = target` if target can be achieved by this node. Add the `frequency` to the result.

1. Remember to remove 1 `currPathSum` from the cache when switching to another branch.

Complexity: O(n)

```python
def pathSum(self, root, target):
    # define global result and path
    self.result = 0
    cache = {0:1}
    
    # recursive to get result
    self.dfs(root, target, 0, cache)
    
    # return result
    return self.result
    
def dfs(self, root, target, currPathSum, cache):
    # exit condition
    if root is None:
        return  
    # calculate currPathSum and required oldPathSum
    currPathSum += root.val
    oldPathSum = currPathSum - target
    # update result and cache
    self.result += cache.get(oldPathSum, 0)
    cache[currPathSum] = cache.get(currPathSum, 0) + 1
    
    # dfs breakdown
    self.dfs(root.left, target, currPathSum, cache)
    self.dfs(root.right, target, currPathSum, cache)
    # when move to a different branch, the currPathSum is no longer available, hence remove one. 
    cache[currPathSum] -= 1
```