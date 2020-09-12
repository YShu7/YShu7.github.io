---
title: Bulls and Cows
tags: [LeetCode]
---

[299. Bulls and Cows](https://leetcode.com/problems/bulls-and-cows/)
#### Solution  
1. `cow` is related with the number of correct characters. If `bull` has used up the corrected characters, 
a misplaced correct character is not considered as `cow`.

1. Count the number of `bull`.

1. Count the sum of **matched** correct characters.

1. `cow = matched - bull`
```python
def getHint(self, secret, guess):
    """
    :type secret: str
    :type guess: str
    :rtype: str
    """
    bull, cow = 0, 0
    dic = collections.Counter(secret)

    for x, y in zip(secret, guess):
        if x == y:
            bull += 1
    cow = sum((dic & collections.Counter(guess)).values()) - bull
    return '{}A{}B'.format(bull, cow)
```