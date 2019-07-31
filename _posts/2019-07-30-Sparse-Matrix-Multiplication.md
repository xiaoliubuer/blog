---
layout: post
title:  "311. Sparse Matrix Multiplication"
date: 2019-07-30 08:29:00 -0400
categories: articles
---
Given two sparse matrices A and B, return the result of AB.
You may assume that A's column number is equal to B's row number.
Example:
```
Input:

A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]
```
Output:
```
     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |
```
```c++
class Solution {
public:
    vector<vector<int>> multiply(vector<vector<int>>& A, vector<vector<int>>& B) {
        if (A.empty() || B.empty()) return {};
        int m = A.size();
        int n = B[0].size();
        vector<vector<int>> res(m, vector<int>(n, 0));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < A[0].size(); ++j) {
                if (!A[i][j]) continue;
                for (int k = 0; k < n; ++k) {
                    if (B[j][k])
                        res[i][k] += A[i][j]*B[j][k];
                }
            }
        }
        return res;
    }
};
```