---
layout: post
title:  "63. Unique Paths II"
date: 2019-01-10 19:56:23 -0400
categories: articles
---
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

Note: m and n will be at most 100.

Example 1:
```
Input:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
Output: 2
Explanation:
There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```
# Function signature
```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        
    }
};
```
# 题意
在前一题基础上加了障碍，障碍要绕开。
# 想法
那就是碰到1就把那里设置成0.
# 参考答案
```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int> >& obstacleGrid) {
	int m = obstacleGrid.size();
	int n = obstacleGrid[0].size();
	if ( m == 0 || n == 0)
		return 0;

	int map[m][n];

	for (int i = 0; i < m; ++i){
		map[i][0] = (obstacleGrid[i][0] == 1) ? 0 : 1;
        if (i > 0 && map[i-1][0] == 0)
            map[i][0] = 0;
        
	}

	for (int i = 0; i < n; ++i){
		map[0][i] = (obstacleGrid[0][i] == 1 ) ? 0 : 1;
        if (i > 0 && map[0][i-1] == 0)
            map[0][i] = 0;
	}

	for (int i = 1; i < m; ++i){
		for (int j = 1; j < n; ++j){
			if (obstacleGrid[i][j] == 1)
				map[i][j] = 0;
			else
				map[i][j] = map[i-1][j] + map[i][j-1];
		}
	}
	return map[m-1][n-1];
    }
};
```