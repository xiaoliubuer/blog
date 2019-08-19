---
layout: post
title:  "695. Max Area of Island"
date: 2019-08-18 18:13:00 -0400
categories: articles
---
Given a non-empty 2D array grid of 0's and 1's, an island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

Example 1:
```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
Given the above grid, return 6. Note the answer is not 11, because the island must be connected 4-directionally.
```
Example 2:
```
[[0,0,0,0,0,0,0,0]]
Given the above grid, return 0.
```
Note: The length of each dimension in the given grid does not exceed 50.

```c++
class Solution {
public:
    int ans =0 , cnt, n ,m;  
    void dfs (vector<vector<int>>& grid  , int i , int j   )
    {
        if(i<0 ||i>=n ||j<0 ||j>=m)
            return ;
        if (grid[i][j]==0)
            return ;
        cnt ++;
        grid[i][j]=0;
        ans = max (ans , cnt );
        dfs(grid , i+1 , j );
        dfs(grid , i-1 , j );
        dfs(grid , i , j+1 );
        dfs(grid , i , j-1 );
        
    }
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        
        n = grid.size();
        if (n==0)
            return 0;
        m = grid[0].size();
        for (int i =0;i<n;i++)for (int j =0;j<m;j++)
        {
            if (grid[i][j]==1)
            {
                cnt =0;
                dfs (grid , i , j );
            }
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        if (grid.empty() || grid[0].empty()) {
            return 0;    
        }        
        max_area = 0;
        int m = grid.size(), n = grid[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int cur_max = 0;
                if (!grid[i][j]) {
                    continue;    
                }
                dfs(grid, i, j, m, n, cur_max);
                max_area = max(max_area, cur_max);
            }   
        }
        return max_area;
    }

    void dfs(vector<vector<int>> &grid, int i, int j, int m, int n, int &cnt) {
        if (i < 0 || i >= m || j < 0 || j >= n || !grid[i][j]) {
            return;
        }
        grid[i][j] = 0;
        cnt++;
        dfs(grid, i - 1, j, m, n, cnt);
        dfs(grid, i + 1, j, m, n, cnt);
        dfs(grid, i, j - 1, m, n, cnt);
        dfs(grid, i, j + 1, m, n, cnt);
    }

private:
    int max_area;
};
```
```c++
class Solution {
public:
    int findArea(vector<vector<int>>& grid, int r, int c){
        
        if(r<0 || c<0 || r>=grid.size() || c>= grid[0].size() || grid[r][c]==0) return 0;
        
        int cur=0;
        
        grid[r][c]=0;
        cur+=findArea(grid, r+1, c);
        cur+=findArea(grid, r-1, c);
        cur+=findArea(grid, r, c-1);
        cur+=findArea(grid, r, c+1);
        
        
        return cur+1;
    }
    
    
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        
        
        int m=grid.size();
        if(m==0) return 0;
        int n=grid[0].size();
        int cur=1;
        int maxArea=INT_MIN;
        for(int i=0;i<m; ++i){
            
            for(int j=0; j<n; ++j){
                
                if(grid[i][j]==1){
                    
                    maxArea=max(maxArea, findArea(grid, i, j));
                }
            }
        }
        
        if(maxArea==INT_MIN) return 0;
        return maxArea;
    }
};
```
```c++
class Solution {
public:
    
    int vis[51][51];
    int mx = 0;
    
    void dfs(int x, int y, int n, int m, vector<vector<int>> &grid, int &count){
        
        if(x < 0 or y < 0 or x >= n or y >= m or grid[x][y] == 0 or vis[x][y] == 1)
            return;
        
        ++count;
        mx = max(mx,count);
        vis[x][y] = 1;
        
        dfs(x-1,y,n,m,grid,count);
        dfs(x,y-1,n,m,grid,count);
        dfs(x+1,y,n,m,grid,count);
        dfs(x,y+1,n,m,grid,count);
    }
    
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        
        int n = grid.size();
        int m = grid[0].size();
        
        int count = 0;
        
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(!vis[i][j] and grid[i][j]){
                    count = 0;
                    dfs(i,j,n,m,grid,count);
                }
            }
        }
        return mx;
    }
};
```