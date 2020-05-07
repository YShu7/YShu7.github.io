---
title: Add and Search Word - Data structure design
tags: LeetCode
---

[211. Add and Search Word - Data structure design](https://leetcode.com/problems/add-and-search-word-data-structure-design/)

#### Solutions
Store words in dictionary whose key is length of words, and value is a list of words.  
> `search()` requires length to be known, we can use this information to narrow down the search space
>to words with the required length.  