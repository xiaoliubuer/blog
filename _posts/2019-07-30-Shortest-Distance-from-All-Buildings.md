---
layout: post
title:  "317. Shortest Distance from All Buildings"
date: 2019-07-30 08:52:00 -0400
categories: articles
---

You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values 0, 1 or 2, where:

Each 0 marks an empty land which you can pass by freely.
Each 1 marks a building which you cannot pass through.
Each 2 marks an obstacle which you cannot pass through.
Example:
```
Input: [[1,0,2,0,1],[0,0,0,0,0],[0,0,1,0,0]]

1 - 0 - 2 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0
```
Output: 7 
```
Explanation: Given three buildings at (0,0), (0,4), (2,2), and an obstacle at (0,2),
             the point (1,2) is an ideal empty land to build a house, as the total 
             travel distance of 3+3+1=7 is minimal. So return 7.
```
Note:
There will be at least one building. If it is not possible to build such house according to the above rules, return -1.

```c++
/*
本题采用bfs的方法，记录每个起点到每个空点的距离，并且相加起来，找到最小值即可
*/
class Solution {
public:
    int shortestDistance(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size(), mark = 0, ans;
        vector<vector<int>> dist(m, vector<int>(n, 0));
        int dirs[5] = {0, 1, 0, -1, 0}; // up, right, down, left
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (grid[i][j] == 1) { //bfs for each building (with value 1)
                    queue<pair<int, int>> bfs;
                    bfs.push({i, j});
                    int level=1;
                    ans=INT_MAX;
                    while (!bfs.empty()) { //Use BFS to extend each building
                        int size = bfs.size();
                        for(int k = 0; k < size; ++k){
                            auto& p = bfs.front();
                            int x = p.first, y = p.second;
                            for (int d = 0; d < 4; d++) {
                                int nx = x + dirs[d], ny = y + dirs[d + 1];
                                if (0 <= nx && nx < m && 0 <= ny && ny < n && grid[nx][ny] == mark) {
                                    grid[nx][ny] = mark - 1; //update the visited map
                                    dist[nx][ny] += level; //level is disct from current checking point, accumulation
                                    ans = min(ans, dist[nx][ny]);
                                    bfs.push({nx, ny});
                                }
                            }
                            bfs.pop();
                        }
                        level++;
                    }
                    if(ans == INT_MAX) return -1;
                    mark -= 1;
                }
        return ans;
    }
};
```
```c++
class Solution {
public:
    int shortestDistance(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size(), mark = 0, ans;
        int dist[m][n] = {0};
        memset(dist, 0, sizeof(int) * m * n);
        int dirs[5] = {0, 1, 0, -1, 0}; // up, right, down, left
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (grid[i][j] == 1) { //bfs for each building (with value 1)
                    queue<pair<int, int>> bfs;
                    bfs.push({i, j});
                    int level=1;
                    ans=INT_MAX;
                    while (!bfs.empty()) { //Use BFS to extend each building
                        int size = bfs.size();
                        for(int k = 0; k < size; ++k){
                            auto& p = bfs.front();
                            int x = p.first, y = p.second;
                            for (int d = 0; d < 4; d++) {
                                int nx = x + dirs[d], ny = y + dirs[d + 1];
                                if (0 <= nx && nx < m && 0 <= ny && ny < n && grid[nx][ny] == mark) {
                                    grid[nx][ny] = mark - 1; //update the visited map
                                    dist[nx][ny] += level; //level is disct from current checking point, accumulation
                                    ans = min(ans, dist[nx][ny]);
                                    bfs.push({nx, ny});
                                }
                            }
                            bfs.pop();
                        }
                        level++;
                    }
                    if(ans == INT_MAX) return -1;
                    mark -= 1;
                }
        return ans;
    }
};
```