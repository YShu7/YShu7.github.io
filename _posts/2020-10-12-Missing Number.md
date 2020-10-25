---
title: Missing Number
tags: [LeetCode]
---

[268. Missing Number](https://leetcode.com/problems/missing-number/)
#### Solution  
1. Find the real number `index` in the arr. 

1. Set the number at the `arr[index]` to a updated number, 
which contains both the information that it is set and its original number.

1. Find the unset number.
```python
def missingNumber(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    N = len(nums)
    # print('new')
    for n in nums:
        idx = (n / 10) - N if n > N else n
        # print(idx)
        if idx == 0:
            continue
        nums[idx - 1] = (nums[idx - 1] + N) * 10
    # print(nums)
    for i, n in enumerate(nums):
        if n <= N:
            return i + 1
    return 0
```
#### Solution - XOR
[Solution](https://leetcode.com/problems/missing-number/discuss/69791/4-Line-Simple-Java-Bit-Manipulate-Solution-with-Explaination)
1. We all know that a^b^b =a, which means two xor operations with the same number will eliminate the number and reveal the original number.

1. Apply XOR operation to both the index and value of the array.
    > In a complete array with no missing numbers, the index and value should be perfectly corresponding( nums[index] = index), so in a missing array, what left finally is the missing number.
```java
public int missingNumber(int[] nums) {

    int xor = 0, i = 0;
	for (i = 0; i < nums.length; i++) {
		xor = xor ^ i ^ nums[i];
	}

	return xor ^ i;
}
```