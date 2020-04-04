---
title: Serialize and Deserialize Binary Tree
tags: LeetCode
---

[297. Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

#### Solutions
**Serialize**  
Print the tree in pre-order traversal. Explicitly noted down `null` nodes.  
**Deserialize**  
Use queue to deserialize the pre-order traversal. 
Since `null` node is stored, we know we should stop when `null` is encountered.