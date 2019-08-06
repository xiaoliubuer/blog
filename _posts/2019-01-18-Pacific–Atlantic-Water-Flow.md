---
layout: post
title:  ". 417. Pacific Atlantic Water Flow"
date: 2019-01-18 22:03:23 -0400
categories: articles
---
Given an m x n matrix of non-negative integers representing the height of each unit cell in a continent, the "Pacific ocean" touches the left and top edges of the matrix and the "Atlantic ocean" touches the right and bottom edges.

Water can only flow in four directions (up, down, left, or right) from a cell to another one with height equal or lower.

Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.

Note:
The order of returned grid coordinates does not matter.
Both m and n are less than 150.
Example:
```
Given the following 5x5 matrix:

  Pacific ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * Atlantic

Return:

[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (positions with parentheses in above matrix).
```
# Function signature
```c++
class Solution {
public:
    vector<pair<int, int>> pacificAtlantic(vector<vector<int>>& matrix) {
    	vector<pair<int, int>> res;
    	return res;
    }
};
```
# 题意
在这个矩阵里每个点表示的是这个位置的高度，矩阵的l，t是太平洋，d，r是大西洋。水可以往上下左右四个方向走，但只能从高往低流。从这矩阵里找出所可以从这个点流到太平洋和大西洋的点。
# 想法
简单直观来说就是DFS。但这道题tricky的地方在于，要满足两个方向。那么两个方向如何才能都满足呢？
# 参考答案
```c++
class Solution {
public:
  int dir[4][2] = { {0, 1}, { 0, -1 }, { 1, 0 }, { -1, 0 } };

  void helper_DFS(vector<vector<int>>& matrix, vector<vector<bool>>& visited, int height, int x, int y){
    int n = matrix.size(), m = matrix[0].size();
    if (x < 0 || x >= n || y < 0 || y >= m || visited[x][y] || matrix[x][y] < height) return;
    visited[x][y] = true;
    for (auto d : dir){
      helper_DFS(matrix, visited, matrix[x][y], x+d[0], y+d[1]);
    }
  }

    vector<pair<int, int>> pacificAtlantic(vector<vector<int>>& matrix) {
        vector<pair<int, int>> res;
        if ( matrix.size() == 0 || matrix[0].size() == 0) return res;

        int n = matrix.size();
        int m = matrix[0].size();

        vector<vector<bool>> patific(n, vector<bool>(m, false));
        vector<vector<bool>> atlantic(n, vector<bool>(m, false));

        for (int i = 0; i < n; ++i){
          helper_DFS(matrix, patific, INT_MIN, i, 0);
          helper_DFS(matrix, atlantic,INT_MIN, i, m - 1);
        }

        for (int i = 0; i < m; ++i){
          helper_DFS(matrix, patific, INT_MIN, 0, i);
          helper_DFS(matrix, atlantic,INT_MIN, n-1,i);
        }

        for (int i = 0; i < n; ++i)
          for (int j = 0; j < m; ++j)
                if (patific[i][j] && atlantic[i][j]) 
                    res.push_back({i, j});
    return res;
    }
};  
```