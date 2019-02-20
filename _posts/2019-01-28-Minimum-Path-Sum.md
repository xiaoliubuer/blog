---
layout: post
title:  "** 64. Minimum Path Sum"
date: 2019-01-28 20:46:23 -0400
categories: articles
---
Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

Example:
```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```
# Function signature
```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        
    }
};
```
# 题意
给一个m * n的无负数矩阵，找到一条从左上到右下的sum最小的路线。
# 想法
创建一个矩阵，没有visited的话值，卧槽，还真的没想到怎么弄诶。
# 尝试解解
```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        
    }
};
```
# Shame answer
```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
    if(grid.empty())
        return 0;

    vector<int> res(grid[0].size(),INT_MAX);
    res[0] = 0;

    for( int i = 0; i < grid.size(); i++ ){
        for( int j = 0; j < grid[0].size(); j++ ){
            if( j > 0 )
                res[j] = min(res[j-1], res[j]) + grid[i][j];
            else
                res[j] = res[j] + grid[i][j];
        }
    }
    return res[grid[0].size() - 1];
}
};
```
```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        if( m == 0 ) return 0;
        int n = grid[0].size();
        for( int i = 1; i < m; i++ ) grid[i][0] += grid[i-1][0];
        for( int j = 1; j < n; j++ ) grid[0][j] += grid[0][j-1];
        for( int i = 1; i < m; i++ ){
            for( int j = 1; j < n; j++ ){
                grid[i][j] += min(grid[i-1][j], grid[i][j-1]);
            }
        }
        return grid[m-1][n-1];
    }
};
```
```c++
class Solution {
public:
int minPathSum(vector<vector>& a) {
    int i, j, k, n = a.size();
    if( !n ) return 0;
    int m = a[0].size();
    for( i = n-1; i >= 0; i-- ){
        for( j = m-1; j >= 0; j-- ){
        	k = INT_MAX;
        	if( j + 1 < m ) k = a[i][j+1];
        	if( i + 1 < n)  k = min(k,a[i+1][j]);
            if( k != INT_MAX ) a[i][j] += k;
        }
    }
    return a[0][0];
}
};
```