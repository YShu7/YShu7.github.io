---
title: Happy Number
tags: [LeetCode]
---

[202. Happy Number](https://leetcode.com/problems/happy-number/)
#### Solution  
1. Use `set` to remember the visited numbers.
```python
def isHappy(self, n):
    """
    :type n: int
    :rtype: bool
    """
    visited = set()
    
    while n not in visited:
        s = 0
        visited.add(n)
        while n != 0:
            s += (n % 10) ** 2
            n = n / 10
        if s == 1:
            return True
        n = s
    return False
```
#### Solution - Floyd Cycle Detection  
[Solution](https://leetcode.com/problems/happy-number/discuss/56917/My-solution-in-C(-O(1)-space-and-no-magic-math-property-involved-))
1. If there is a cycle in the path, can stop to detection already.
```C
int digitSquareSum(int n) {
    int sum = 0, tmp;
    while (n) {
        tmp = n % 10;
        sum += tmp * tmp;
        n /= 10;
    }
    return sum;
}

bool isHappy(int n) {
    int slow, fast;
    slow = fast = n;
    do {
        slow = digitSquareSum(slow);
        fast = digitSquareSum(fast);
        fast = digitSquareSum(fast);
    } while(slow != fast);
    if (slow == 1) return 1;
    else return 0;
}
```