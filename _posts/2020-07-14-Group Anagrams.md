---
title: Group Anagrams
tags: [LeetCode]
---

[49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)
#### Solution 1  
1. Get **sorted** chars array in each str. 

1. Store the strs with the same chars array into the same dictionary item.

```python
def groupAnagrams(self, strs):
    """
    :type strs: List[str]
    :rtype: List[List[str]]
    """
    dic = {}
    for s in strs:
        chars = ''.join(sorted(s))
        if chars in dic:
            dic[chars].append(s)
        else:
            dic[chars] = [s]
    
    res = []
    for key in dic:
        res.append(dic[key])
    return res
```
#### Solution 2
1. Get the number of appearance of each characters in the array.

1. Use the `count` as key.
```python
def groupAnagrams(self, strs):
    ans = collections.defaultdict(list)
    for s in strs:
        ans[tuple(sorted(s))].append(s)
    return ans.values()
```