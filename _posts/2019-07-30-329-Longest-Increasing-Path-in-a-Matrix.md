---
layout: post
title:  "329. Longest Increasing Path in a Matrix"
date: 2019-07-30 19:45:00 -0400
categories: articles
---
Given an integer matrix, find the length of the longest increasing path.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).

Example 1:
```
Input: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
Output: 4 
Explanation: The longest increasing path is [1, 2, 6, 9].
```
Example 2:
```
Input: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
Output: 4 
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
```

```c++
class Solution {
public:
    int helper(vector<vector<int>>& matrix, int x, int y, int cur, vector<vector<int>>& visited){
        if ( x < 0 || x >= matrix.size() || y < 0 || y >= matrix[0].size()) return 0;
        if (matrix[x][y] <= cur) return 0;
        if (visited[x][y] > 0 ) return visited[x][y];
        int u = 0, d = 0, l = 0, r = 0;
        u = helper(matrix, x-1, y, matrix[x][y], visited);
        d = helper(matrix, x+1, y, matrix[x][y], visited);
        l = helper(matrix, x, y-1, matrix[x][y], visited);
        r = helper(matrix, x, y+1, matrix[x][y], visited);
        visited[x][y] = max(u, max(d, max(l, r)))+1;
        return visited[x][y];
    }
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        int res = 0;
        if (matrix.empty() || matrix[0].empty()) return res;
        vector<vector<int>> visited(matrix.size(), vector<int>(matrix[0].size(), 0));
        for (int i = 0; i < matrix.size(); i++){
            for (int j = 0; j < matrix[0].size(); j++){
                res = max(res, helper(matrix, i, j, INT_MIN, visited));
            }
        }
        return res;
    }
};
```