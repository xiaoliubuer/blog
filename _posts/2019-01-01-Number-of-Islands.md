---
layout: post
title:  "200. Number of Islands"
date: 2019-01-01 10:52:23 -0400
categories: articles
---
Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example 1:
```
Input:
11110
11010
11000
00000

Output: 1
```
Example 2:
```
Input:
11000
11000
00100
00011

Output: 3
```
# 题意
一个2D矩阵map，1是地，0是水，计算这个map中有多少个岛。只要横向或纵向相连就算一块大陆。
# Funciton signature
```c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        
    }
};
```
# 思路
1. 要有一个循环来遍历每个点。
2. 遍历到每个点后，如果这个点是1，那么要对这个点进行DFS, 找到这个点所在的大陆的所有的点。并且标注这些点已经都被访问过了。
3. 只要发现一个点是没有被访问过的，那么把counter++。

# 参考答案
```c++
// Accepted!!
class Solution {
public:
	void helper(vector<vector<char>>& grid, int x, int y){
		grid[x][y] = '0';
		if ( x - 1 >= 0 && grid[x-1][y] == '1') helper( grid, x-1, y );
		if ( x + 1 < grid.size() && grid[x+1][y] == '1') helper( grid, x+1, y );
		if ( y - 1 >= 0 && grid[x][y-1] == '1') helper( grid, x, y-1 );
		if ( y + 1 < grid[0].size() && grid[x][y+1] == '1') helper( grid, x, y+1 );
		return;
	}
    int numIslands(vector<vector<char>>& grid) {
    	if ( grid.size() == 0 || grid[0].size() == 0) return 0;
    	int res = 0;
    	for (int i = 0; i < grid.size(); i++) {
    		for (int j = 0; j < grid[0].size(); j++){
    			if (grid[i][j] == '1') {
    				helper(grid, i, j);
    				res++;
    			}
    		}
    	}
    	return res;
    }
};
```