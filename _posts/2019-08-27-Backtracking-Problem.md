---
title: Backtracking Problems
tags: LeetCode
---

#### Features
- Need to make choices
- Have constraints
- To achieve a goal

#### Examples
- N Queens
- Permutation

#### Functions
1. driver function
2. solve function
To help with making choices, exploring, and undo choices
3. validating function
To help with exploring whether the choice is valid

#### Things we need to do
- Find out **base case**
- Find out **constraints**, i.e. when we change choices
- Find **goal**, i.e. when we stop
- **Store the state** before making a choice, and go back to it