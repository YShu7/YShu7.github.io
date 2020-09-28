---
title: Check If String Is Transformable With Substring Sort Operations
tags: [LeetCode]
---

[1585. Check If String Is Transformable With Substring Sort Operations](https://leetcode.com/problems/check-if-string-is-transformable-with-substring-sort-operations/)  
#### Solution  
[Solution](https://leetcode.com/problems/check-if-string-is-transformable-with-substring-sort-operations/discuss/843954/Python-short-solution-detailed-comments-O(N)-w-stack-trick)

1. For every digit in `t`, we check if we can put this digit in `s` to the same position as t.

1. If we can't find the digit, just return false.

1. If we found the digit, we must make sure all the left digits are **equal or greater than** it.
> We can at least move it to left one by one position until to the same position as in `t`.
> Always visiting the first occurrence of digit in `s` can fasten the process, as we only don't expect to update the visited part again.

```python
def isTransformable(self, s, t):
    """
    :type s: str
    :type t: str
    :rtype: bool
    """
    # Use stack to check from left to right
    dic = collections.defaultdict(list)
    for i in reversed(range(len(s))):
        c = int(s[i])
        dic[c].append(i)

    for c in t:
        c = int(c)
        if not dic[c]: #digit is not in s, return False
            return False
        for smaller_c in range(c): #only loop over digits smaller than current digit
            if not dic[smaller_c]:
                continue
            if dic[smaller_c][-1] < dic[c][-1]: #there is a digit smaller than current digit, return false
                return False
        dic[c].pop() # remove the digit as it's visited
    return True
```