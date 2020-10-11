---
title: Maximum Number of Achievable Transfer Requests
tags: [LeetCode]
---

[1601. Maximum Number of Achievable Transfer Requests](https://leetcode.com/problems/maximum-number-of-achievable-transfer-requests/)
We have n buildings numbered from 0 to n - 1. Each building has a number of employees. It's transfer season, and some employees want to change the building they reside in.

You are given an array requests where requests[i] = [fromi, toi] represents an employee's request to transfer from building fromi to building toi.

All buildings are full, so a list of requests is achievable only if for each building, the net change in employee transfers is zero. This means the number of employees leaving is equal to the number of employees moving in. For example if n = 3 and two employees are leaving building 0, one is leaving building 1, and one is leaving building 2, there should be two employees moving to building 0, one employee moving to building 1, and one employee moving to building 2.

Return the maximum number of achievable requests.

#### Solution  
[Solution](https://leetcode.com/problems/maximum-number-of-achievable-transfer-requests/discuss/866456/Python-Check-All-Combinations)
1. For each combination, use a mask to present the picks. The kth bits means we need to satisfy the kth request.

1. If for all buildings, in degrees == out degrees, it's achievable.
```python
def maximumRequests(self, n, requests):
    """
    :type n: int
    :type requests: List[List[int]]
    :rtype: int
    """
    nr = len(requests)
    res = 0

    def test(mask):
        outd = [0] * n
        ind = [0] * n
        for k in xrange(nr):
            if (1 << k) & mask:
                outd[requests[k][0]] += 1
                ind[requests[k][1]] += 1
        return sum(outd) if outd == ind else 0

    for i in xrange(1 << nr):
        res = max(res, test(i))
    return res
```