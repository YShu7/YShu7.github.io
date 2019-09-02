---


title: Kadane's Algorithms
tags: LeetCode
---

#### Features
- Find the largest contiguous elements in an array
- O(n)
- Dynamic Programming

#### Explaination
Assume we're finding the largest contiguous elements Maximum in an array A
1. Optimization
The largest contiguous elements S[i] in A that ends **exactly** with A[i] is 
`S[i]= Max(S[i-1]+A[i], A[i-1])`
i.e. the maximum of the largest contiguous elements S[i-1] in A that ends **exactly** with A[i-1] plus A[i] or A[i] itself (i.e. S[i-1] < 0 and thus is given up)
2. 
To find the largest contiguous elements S in an array A, computes S for each element in A,
the largest of them is what we want.

#### 2-D Version

Find the largest contiguous rectangles in a 2-D array

Assume the following is the array we need to process

![Original Array](/assets/images/kadane-1.png)

Enumerate through the rows. For each pair of row_i to row_j, we compute prefix sum for each column. And then apply Kadane's Algorithm to the result 1-D array.

![Step a](/assets/images/kadane-2.png)

![Step b](/assets/images/kadane-3.png)

![Step d](/assets/images/kadane-4.png)

![Step e](/assets/images/kadane-5.png)

![Step f](/assets/images/kadane-6.png)

![Step g](/assets/images/kadane-7.png)

#### 2-D Version with K

Find the largest contiguous rectangles in a 2-D array which doesn't exceed K

Instead of applying Kadane's Algorithm, we need to compute prefix sum of the 1-D array that ends with col_d now.

![Step 1](/assets/images/kadane-5.png)

![Step 2](/assets/images/kadane-8.png)

Find `M = ceil(S-K)` to find the smallest sub-rectangle to be eliminated. `S - M` is the result.