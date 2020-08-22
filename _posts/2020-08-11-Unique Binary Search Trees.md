---
title: Unique Binary Search Trees
tags: [LeetCode, Dynamic Programming]
---

[96. Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/)
#### Solution  
1. For each number from 0 to n, set it as `pivot`, i.e. root of the tree.

1. Find the number of `left` and `right` tree for each `pivot`.

1. Consider `right` whose range is `[pivot+1, n]` as a tree whose range is `[0, n-pivot-1]`.

1. Use a dictionary to avoid duplicate computing.

```python
def numTrees(self, n):
    """
    :type n: int
    :rtype: int
    """
    self.dic = {0:1, 1:1}
    return self.find_tree(n)

def find_tree(self, n):
    if n in self.dic:
        return self.dic[n]
    num = 0
    for i in range(0, n):
        num += self.find_pivot(n, i)
    self.dic[n] = num
    return num
    
def find_pivot(self, n, pivot):
    l = self.find_tree(pivot)
    r = self.find_tree(n-pivot-1)
    return l * r
```