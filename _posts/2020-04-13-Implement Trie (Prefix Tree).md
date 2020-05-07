---
title: Implement Trie (Prefix Tree)
tags: LeetCode
---

[208. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

#### Solutions
Store words in dictionary whose key is the first character of words, and value is a list of words.  
> `startsWith()` requires the first several characters to be known, we can use this information to narrow down the search space.
> Similar to [211. Add and Search Word - Data structure design](https://leetcode.com/problems/add-and-search-word-data-structure-design/)
