---
layout: post
title:  "905. Sort Array By Parity"
date: 2019-05-22 20:41:00 -0400
categories: articles
---

Given an array A of non-negative integers, return an array consisting of all the even elements of A, followed by all the odd elements of A.

You may return any answer array that satisfies this condition.
Example 1:
```
Input: [3,1,2,4]
Output: [2,4,3,1]
The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.
```
```c++
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& A) {
        int left = 0, right = A.size() - 1;
        
        while ( left < right ) {
            if ( A[left] % 2 == 0 ) left++;
            else if (A[right] % 2 == 1) right--;
            else swap(A[left], A[right]);
        }
        return A;
    }
};
```