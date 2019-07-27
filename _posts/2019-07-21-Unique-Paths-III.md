---
layout: post
title:  "980. Unique Paths III"
date: 2019-07-21 18:22:00 -0400
categories: articles
---
On a 2-dimensional grid, there are 4 types of squares:

1 represents the starting square.  There is exactly one starting square.
2 represents the ending square.  There is exactly one ending square.
0 represents empty squares we can walk over.
-1 represents obstacles that we cannot walk over.
Return the number of 4-directional walks from the starting square to the ending square, that walk over every non-obstacle square exactly once.

 

Example 1:
```
Input: [[1,0,0,0],[0,0,0,0],[0,0,2,-1]]
Output: 2
Explanation: We have the following two paths: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)
```
Example 2:
```
Input: [[1,0,0,0],[0,0,0,0],[0,0,0,2]]
Output: 4
Explanation: We have the following four paths: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)
```
Example 3:
```
Input: [[0,1],[2,0]]
Output: 0
```
Explanation: 
```
There is no path that walks over every empty square exactly once.
Note that the starting and ending square can be anywhere in the grid.
```

Note:

1 <= grid.length * grid[0].length <= 20



```c++
// Accepted!!
class Solution {
public:
    int res = 0;
    int uniquePathsIII(vector<vector<int>>& grid) {
        if ( grid.size() == 0 || grid[0].size() == 0 ) return res;
        int record = 0;
        int x = 0, y = 0;
        for ( int i = 0; i < grid.size(); i++ ) {
            for ( int j = 0; j < grid[0].size(); j++ ) {
                if ( grid[i][j] == 0 ) record++;
                if ( grid[i][j] == 1 ){
                    x = i;
                    y = j;
                }
            }
        }
        vector<vector<bool>> visited(grid.size(), vector<bool>(grid[0].size(), false));
        helper(grid, x, y, visited, record + 1);
        return res;
    }
    
    void helper(vector<vector<int>>& grid, int x, int y, vector<vector<bool>>& visited, int record){
        if (x < 0 || x >= grid.size() || y < 0 || y >= grid[0].size() || grid[x][y] == -1 || visited[x][y] == true || record < 0) return;
        if ( grid[x][y] == 2 && record == 0 ) { 
            res++;
            return;
        } else {
        visited[x][y] = true;
        helper(grid, x-1, y, visited, record - 1);
        helper(grid, x+1, y, visited, record - 1);
        helper(grid, x, y-1, visited, record - 1);
        helper(grid, x, y+1, visited, record - 1);
        visited[x][y] = false;
        }
    }
};
```