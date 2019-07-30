---
layout: post
title:  "304. Range Sum Query 2D - Immutable"
date: 2019-07-29 19:53:00 -0400
categories: articles
---
Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

Range Sum Query 2D
The above rectangle (with the red border) is defined by (row1, col1) = (2, 1) and (row2, col2) = (4, 3), which contains sum = 8.

Example:
```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
```
Note:
You may assume that the matrix does not change.
There are many calls to sumRegion function.
You may assume that row1 ≤ row2 and col1 ≤ col2.


```c++
class NumMatrix {
public:
    NumMatrix(vector<vector<int>>& matrix) {
        if (matrix.size() == 0 || matrix[0].size() == 0) return;
        vector<vector<int>> n(matrix.size()+1, vector<int>(matrix[0].size()+1, 0));
        cache = n;
        
        //cache = new int[matrix.size()+1][matrix[0].size()+1];
        for (int r = 0; r < matrix.size(); r++) {
            for (int c = 0; c < matrix[0].size(); c++) {
                cache[r + 1][c + 1] = cache[r + 1][c] + cache[r][c + 1] + matrix[r][c] - cache[r][c];
            }
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        return cache[row2 + 1][col2 + 1] - cache[row1][col2+1] - cache[row2+1][col1] + cache[row1][col1];
    }
    
    vector<vector<int>> cache;
};
```