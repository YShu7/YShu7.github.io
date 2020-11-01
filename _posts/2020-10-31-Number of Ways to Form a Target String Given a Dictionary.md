---
title: Number of Ways to Form a Target String Given a Dictionary
tags: [LeetCode, Dynamic Programming]
---

[1639. Number of Ways to Form a Target String Given a Dictionary](https://leetcode.com/problems/number-of-ways-to-form-a-target-string-given-a-dictionary/)
You are given a list of strings of the same length words and a string target.

Your task is to form target using the given words under the following rules:

target should be formed from left to right.  
To form the ith character (0-indexed) of target, you can choose the kth character of the jth string in words if `target[i] = words[j][k]`.  
Once you use the kth character of the jth string of words, you can no longer use the xth character of any string in words where x <= k. In other words, all characters to the left of or at index k become unusuable for every string.  
Repeat the process until you form the string target.  
Notice that you can use multiple characters from the same string in words provided the conditions above are met.  

Return the number of ways to form target from words. Since the answer may be too large, return it modulo 10 ** 9 + 7.

#### Solution  
1. Count the number of character `c` at index `i` of all words.

1. `dp[w][t]` represents the number of ways for `target[:t] from words[:w]`.

1. `dp[w][t]` = skip all characters at index `w` + sum of all characters at index `w` that is the same as `target[t]`

```python
def numWays(self, words, target):
    """
    :type words: List[str]
    :type target: str
    :rtype: int
    """

    W, T = len(words[0]), len(target)
    dic = [collections.Counter() for _ in range(W)]
    for word in words:
        for i, c in enumerate(word):
            dic[i][c] += 1
    
    dp = [[0] * (1+T) for _ in range(1+W)]
    for w in range(1,W+1):
        dp[w][1] = dp[w-1][1] + sum([1 if word[w-1] == target[0] else 0 for word in words])
     # number of ways for target[:T] from words[:W]
    for t in range(2,T+1):
        for w in range(t,W+1):
            dp[w][t] += dp[w-1][t] + sum([dp[w-1][t-1] * v if c == target[t-1] else 0 for c, v in dic[w-1].items()])

    return dp[-1][-1] % (10 ** 9 + 7)
```