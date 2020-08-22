---
title: Best Time to Buy and Sell Stock with Cooldown
tags: [LeetCode, Dynamic Programming]
---

[309. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)
#### Solution  
1. There're 3 states, `hold`, `nohold`, 'cooldown'.

1. Keep the max profit up to time `i` when we are in these three states.

1. `hold` can be achieved by `donothing (hold)` or `buy (nohold - p)`

1. `nohold` can be achieved by `donothing (nohold)` or `sell (cooldown, because after we sell sth, we directly enter the cooldown state)`

1. `cooldown` can be achieved by `sell (hold + p)`

1. The initial state is `nohold`.

```python
dic = {}
def maxProfit(self, prices):
    """
    :type prices: List[int]
    :rtype: int
    """
    hold, nohold, cooldown = -float('inf'), 0, -float('inf')
    for p in prices:
        hold, nohold, cooldown = max(hold, nohold - p), max(nohold, cooldown), hold + p
    
    return max(nohold, cooldown)
```