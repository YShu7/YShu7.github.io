---
title: Edit Distance
tags: [LeetCode, Dynamic Programming]
---

[72. Edit Distance](https://leetcode.com/problems/edit-distance/)
#### Solution - DP  
1. Consider computing the distance between sub-strings, so that we only need to consider about the last character.

1. The delete and insert operations are sure to make some differences.

1. The replace operation will **NOT** make any difference when the last character is the same.

1. An extra col and row are needed for the empty string, i.e. base cast.

```python
def minDistance(self, word1, word2):
    """
    :type word1: str
    :type word2: str
    :rtype: int
    """
    m, n = len(word1), len(word2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    src = [[''] * (n + 1) for _ in range(m + 1)]
    dst = [[''] * (n + 1) for _ in range(m + 1)]
    
    for row in range(1, m+1):
        dp[row][0] = dp[row - 1][0] + 1
        src[row][0] = word1[:row]
    for col in range(1, n+1):
        dp[0][col] = dp[0][col - 1] + 1
        dst[0][col] = word2[:col]
    for row in range(1, m+1):
        for col in range(1, n+1):
            dp[row][col] = min(dp[row - 1][col] + 1,
                               dp[row][col - 1] + 1,
                               dp[row - 1][col - 1] + (1 if word1[row-1] != word2[col-1] else 0))
    return dp[-1][-1]
```