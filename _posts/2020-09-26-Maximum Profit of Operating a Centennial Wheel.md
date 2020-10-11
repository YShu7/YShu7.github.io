---
title: Maximum Profit of Operating a Centennial Wheel
tags: [LeetCode]
---

[1599. Maximum Profit of Operating a Centennial Wheel](https://leetcode.com/problems/maximum-profit-of-operating-a-centennial-wheel/)

You are the operator of a Centennial Wheel that has four gondolas, and each gondola has room for up to four people. You have the ability to rotate the gondolas counterclockwise, which costs you runningCost dollars.

You are given an array customers of length n where customers[i] is the number of new customers arriving just before the ith rotation (0-indexed). This means you must rotate the wheel i times before the customers[i] customers arrive. You cannot make customers wait if there is room in the gondola. Each customer pays boardingCost dollars when they board on the gondola closest to the ground and will exit once that gondola reaches the ground again.

You can stop the wheel at any time, including before serving all customers. If you decide to stop serving customers, all subsequent rotations are free in order to get all the customers down safely. Note that if there are currently more than four customers waiting at the wheel, only four will board the gondola, and the rest will wait for the next rotation.

Return the minimum number of rotations you need to perform to maximize your profit. If there is no scenario where the profit is positive, return -1.

#### Solution  

1. Greedily select `max(people, 4)` to on board.

1. Compute the current profit.

1. If the profit is larger, update maximum profit.

```python
def minOperationsMaxProfit(self, customers, boardingCost, runningCost):
    """
    :type customers: List[int]
    :type boardingCost: int
    :type runningCost: int
    :rtype: int
    """
    max_profit, rotation = 0, 0
    
    curr_c, queue = 0, []
    for c in customers:
        curr_c += c
        queue_out = min(max(curr_c, 0), 4)
        queue.append(queue_out)
        curr_c -= queue_out
    while curr_c > 0:
        queue_out = min(max(curr_c, 0), 4)
        queue.append(queue_out)
        curr_c -= queue_out
    # print(queue)
    
    for i, q in enumerate(queue, 1):
        if max_profit + q * boardingCost - runningCost > max_profit:
            max_profit += q * boardingCost - runningCost
            rotation = i
    
    return rotation if max_profit > 0 else -1
```