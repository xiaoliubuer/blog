---
layout: post
title:  "463. Island Perimeter"
date: 2019-08-05 19:39:00 -0400
categories: articles
---
You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water.

Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

Example:
```
Input:
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]
```
Output: 16
```
Explanation: The perimeter is the 16 yellow stripes in the image below:
```
```c++
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        int repeat = 0, count = 0;
        for (int i = 0; i < grid.size(); ++i)
        {
            for (int j = 0; j < grid[i].size(); ++j)
            {
                if (grid[i][j] == 1)
                {
                    ++count;
                    if (i != 0 && grid[i-1][j] == 1) ++repeat;
                    if (j != 0 && grid[i][j-1] == 1) ++repeat;
                }
            }
        }
        return 4*count - 2*repeat;
    }
};
```
```c++
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        int n=grid.size();
        int m=grid[0].size();
        int ans=0;
        int repeat=0;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(grid[i][j]==1)
                {
                ans++;
                if(i<n-1 && grid[i+1][j] == 1) repeat++;
                if(j<m-1 && grid[i][j+1] == 1) repeat++;
                }
            }
        }
        
        return 4*ans-2*repeat;
    }
};
```
```c++
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        int count = 0;
        int repeat = 0;
        
        for (int i = 0; i < grid.size(); ++i)
        {
            for (int j = 0; j < grid.front().size(); ++j)
            {
                if (not grid.at(i).at(j)) continue;
                count += 1;
                if (i != 0 and grid.at(i - 1).at(j)) repeat += 1;
                if (j != 0 and grid.at(i).at(j - 1)) repeat += 1;
            }
        }
        
        return count * 4 - repeat * 2;
    }
};
```
```c++
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        int ans = 0;
        for (int i=0; i<grid.size(); i++) {
            for (int j=0; j<grid[i].size(); j++) {
                if (grid[i][j] == 0) continue;
                if (i - 1 < 0 || grid[i-1][j] == 0) ans++;
                if (i + 1 >= grid.size() || grid[i+1][j] == 0) ans++;
                if (j - 1 < 0 || grid[i][j-1] == 0) ans++;
                if (j + 1 >= grid[i].size() || grid[i][j+1] == 0) ans++;
            }
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        if (grid.size() == 0 || grid[0].size() == 0){
        	return 0;
        }

        int res = 0;
        int m = grid.size(), n = grid[0].size();

        for (int i = 0; i < m; ++i){
        	for (int j = 0; j < n; ++j){
        		if ( grid[i][j] == 0 ) continue;
        		res += 4;

        		if ( i > 0 && grid[i - 1][j] == 1 ) res -= 2;
        		if ( j > 0 && grid[i][j - 1] == 1 ) res -= 2;
        	}
        }
 	return res;
    }
};
```