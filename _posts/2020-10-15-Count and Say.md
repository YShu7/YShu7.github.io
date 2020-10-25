---
title: Count and Say
tags: [LeetCode]
---

[38. Count and Say](https://leetcode.com/problems/count-and-say/)
#### Solution  
[Solution](https://leetcode.com/problems/count-and-say/discuss/15999/4-5-lines-Python-solutions)
```python
def countAndSay(self, n):
    s = '1'
    for _ in range(n - 1):
        s = ''.join(str(len(list(group))) + digit
                    for digit, group in itertools.groupby(s))
    return s
```