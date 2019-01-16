---
layout: post
title:  "62. Unique Paths"
date: 2019-01-10 19:42:23 -0400
categories: articles
---
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

Above is a 7 x 3 grid. How many possible unique paths are there?

Note: m and n will be at most 100.

Example 1:
```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
```
Example 2:
```
Input: m = 7, n = 3
Output: 28
```
# Function signature
```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        
    }
};
```
# 题意
一个在左上角的机器人走到右下角有多少种走法？只能往下或往右走。
# 想法
DP，创建一个同样大小的矩阵，最上边都设成1，最左边也都设成1，因为每一步的确只有一种方法。然后计算其余的点。
从[1,1]开始遍历，这个点就等于它上面⬆️的点和它左边⬅️的点的可能性的加和。
直到最后，就是所有的可能行。
# 尝试解答
```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
    	if ( m == 0 || n == 0 ) return 0;
    	int pathSum[m][n] = {0};
    	for (int i = 0; i < n; ++i){
    		pathSum[0][i] = 1;
    	}
    	for (int i = 0; i < m; ++i){
    		pathSum[i][0] = 1;
    	}

    	for (int i = 1; i < m; ++i){
    		for (int j = 1; j < n; ++j){
    			pathSum[i][j] = pathSum[i-1][j] + pathSum[i][j-1];
    		}
    	}
        return pathSum[m-1][n-1];
    }
};
```