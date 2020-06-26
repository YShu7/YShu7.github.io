---
title: Best Time to Buy and Sell Stock
tags: LeetCode [Dynamic Programming], [Kadane's Algorithm]
---

[121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
#### Solution 1 - DP
1. Keep the **seen** minimum buy price and check if sell at current price can get more profit.
> We do not get the minimum value for the whole list first because we can only sell after buy.
> So [2, 4, 5, 1], when we are at 5 we can only get 3 because 2 is the minimum price up to now.

#### Solution 2 - Kadane
[Soluion](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/discuss/39038/Kadane's-Algorithm-Since-no-one-has-mentioned-about-this-so-far-%3A\)-\(In-case-if-interviewer-twists-the-input\))
1. Use the equation that (a3 - a2) + (a2 - a1) = a3 - a1

1. So that we can accumulate the profit between each slot to get the total profit.

1. In this case, we only accumulate the previous profit when it is **positive**, i.e. it can make contribution to the next batch.

1. Therefore we reset to 0 when accumulated profit < 0.
> The idea is similar to [53. Maximum Subarray](2020-06-26-Maximum%20Subarray.md)


If we can continuously buy and sell so that we can earn more:
1. Get the profit if we buy at the previous timeslot and sell now.  
1. If we can earn money, do this. Else do nothing, so profit = 0.  