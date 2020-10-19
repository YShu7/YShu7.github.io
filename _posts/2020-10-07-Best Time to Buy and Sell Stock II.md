---
title: Best Time to Buy and Sell Stock II
tags: [LeetCode]
---

[122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
#### Solution  
1. Keep a strictly increasing stack.

1. When a decreased element come in, add the profit that can be earned from the stack, and clean up the stack.

```python
def maxProfit(self, prices):
    """
    :type prices: List[int]
    :rtype: int
    """
    stack = []
    profit = 0
    for p in prices:
        if stack:
            if stack[-1] <= p:
                stack.append(p)
            else:
                profit += stack[-1] - stack[0]
                stack = [p]
        else:
            stack = [p]
    if stack:
        profit += stack[-1] - stack[0]
    return profit
```