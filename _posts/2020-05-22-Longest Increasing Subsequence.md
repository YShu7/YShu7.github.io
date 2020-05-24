---
title: Longest Increasing Subsequence
tags: LeetCode, [Dynamic Programming]
mathjax: true
---

[300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

#### Solution 1 - Recursion with Memoization
1. Remove redudancy by store information in `memo`.  
> `memo`: `memo[i][j]` represents the length of the LIS possible using `nums[i]` as the **previous** element considered 
> to be included/not included in the LIS, with `nums[j]` as the **current** element considered to be included/not included in the LIS.  
2. For each pair of `prev` and `curr`, consider either take `curr` or not.

Time Complexity: $O(n^2)$
Space Complexity: $O(n^2)$
#### Solution 2 - Dynamic Programming
The longest increasing subsequence possible upto the $i^{th}$ index in a given array is independent of the elements coming later on in the array.
1. Store the length of LIS upto $i^th$ element into an array `dp`.  
> `dp[i]` represents the length of the LIS possible considering the array elements upto the $i^{th}$ index only,
>by necessarily including the $i^{th}$ element.   
2. Loop through all $j < i$ to find the LIS for $j$ which results in LIS for $i$.  
Time Complexity: $O(n^2)$
Space Complexity: $O(n)$
#### Solution 3 - Dynamic Programming with Binary Search
We only care about the length of the LIS instead of the sequence. 
Store the elements in an increasing order (with some conditions) to make it 'least constraining' for LIS.
https://algorithmsandme.com/longest-increasing-subsequence-in-onlogn/  
1. Store an increasing subsequence formed by including the currently encountered element.  
1. If the element > all visited elements: append it to the end of the sequence.
1. If the element <= some elements: replace the smallest element that is > it by it.
