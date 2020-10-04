---
title: Rearrange Spaces Between Words
tags: [LeetCode]
---

[1592. Rearrange Spaces Between Words](https://leetcode.com/problems/rearrange-spaces-between-words/)

You are given a string text of words that are placed among some number of spaces. Each word consists of one or more lowercase English letters and are separated by at least one space. It's guaranteed that text contains at least one word.

Rearrange the spaces so that there is an equal number of spaces between every pair of adjacent words and that number is maximized. If you cannot redistribute all the spaces equally, place the extra spaces at the end, meaning the returned string should be the same length as text.

Return the string after rearranging the spaces.

#### Solution  
1. Get the number of spaces and words.

1. Calculate the number of spaces between each word.

1. Append andy remaining space to the end.

```python
def reorderSpaces(self, text):
    """
    :type text: str
    :rtype: str
    """
    words = text.split()
    num_words = len(words)
    num_spaces = len(text) - len(''.join(words))
    
    if num_words == 1:
        return words[0] + ' ' * num_spaces
    
    div, mod = divmod(num_spaces, num_words - 1)
    spliter = ' ' * div
    
    res = ''
    for i in range(num_words-1):
        res += words[i]
        res += spliter
    res += words[-1]
    res += ' ' * mod
    
    return res
```