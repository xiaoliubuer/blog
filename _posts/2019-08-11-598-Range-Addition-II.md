---
layout: post
title:  "598. Range Addition II"
date: 2019-08-11 18:16:00 -0400
categories: articles
---	
Given an m * n matrix M initialized with all 0's and several update operations.

Operations are represented by a 2D array, and each operation is represented by an array with two positive integers a and b, which means M[i][j] should be added by one for all 0 <= i < a and 0 <= j < b.

You need to count and return the number of maximum integers in the matrix after performing all the operations.

Example 1:
```
Input: 
m = 3, n = 3
operations = [[2,2],[3,3]]
Output: 4
```
Explanation: 
```
Initially, M = 
[[0, 0, 0],
 [0, 0, 0],
 [0, 0, 0]]

After performing [2,2], M = 
[[1, 1, 0],
 [1, 1, 0],
 [0, 0, 0]]

After performing [3,3], M = 
[[2, 2, 1],
 [2, 2, 1],
 [1, 1, 1]]
```
So the maximum integer in M is 2, and there are four of it in M. So return 4.
Note:
```
The range of m and n is [1,40000].
The range of a is [1,m], and the range of b is [1,n].
The range of operations size won't exceed 10,000.
```
```c++
class Solution {
public:
    int maxCount(int m, int n, vector<vector<int>>& ops) {
        for(int i=0;i<ops.size();i++)
        {
            m = min(ops[i][0],m);
            n = min(ops[i][1],n);
        }
        return m*n;
    }
};
```
```c++
class Solution {
public:
    int maxCount(int m, int n, vector<vector<int>>& ops) {
        for(const auto& o : ops) {
            m = std::min(m, o[0]);
            n = std::min(n, o[1]);
        }
        
        return m * n;
    }
};
```
```c++
class Solution {
public:
    int maxCount(int m, int n, vector<vector<int>>& ops) {
        int minI = 40000;
        int minJ = 40000;
        
        if (ops.size() == 0)
            return m * n;
        
        for (vector<vector<int>>::iterator it = ops.begin(); it != ops.end(); it++) {
            int i = (*it)[0];
            int j = (*it)[1];
            
            if (i < minI) {
                minI = i;
            }
            
            if (j < minJ) {
                minJ = j;
            }
        }
        
        return minI * minJ;
    }
};
```