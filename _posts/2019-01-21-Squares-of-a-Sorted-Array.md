---
layout: post
title:  "977. Squares of a Sorted Array"
date: 2019-01-21 17:41:23 -0400
categories: articles
---
Given an array of integers A sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.

Example 1:
```
Input: [-4,-1,0,3,10]
Output: [0,1,9,16,100]
```
Example 2:
```
Input: [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

Note:
```
1 <= A.length <= 10000
-10000 <= A[i] <= 10000
A is sorted in non-decreasing order.
```
# Function signature
```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& A) {
        
    }
};
```
# 题意

# 想法

# 尝试解解
```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& A) {
        sort(A.begin(), A.end(), [](int& a, int& b){
            return a*a < b*b;
        });
        for ( int i = 0; i < A.size(); i++ ) 
            A[i] = A[i]*A[i];
        return A;
    }
};
```