---
layout: post
title:  "286. Walls and Gates"
date: 2019-05-22 21:10:00 -0400
categories: articles
---


You are given a m x n 2D grid initialized with these three possible values.

-1 - A wall or an obstacle.
0 - A gate.
INF - Infinity means an empty room. We use the value 231 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.
Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

Example: 
```
Given the 2D grid:

INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
After running your function, the 2D grid should be:

  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
```
```c++
void wallsAndGates(vector<vector<int>>& rooms) {
    const int row = rooms.size();
    if (0 == row) return;
    const int col = rooms[0].size();
    queue<pair<int, int>> canReach;  // save all element reachable
    vector<pair<int, int>> dirs = { {1, 0}, {0, 1}, {-1, 0}, {0, -1} }; // four directions for each reachable
    for(int i = 0; i < row; i++){
        for(int j = 0; j < col; j++){
            if(0 == rooms[i][j])
                canReach.emplace(i, j);
        }
    }
    while(!canReach.empty()){
        int r = canReach.front().first, c = canReach.front().second;
        canReach.pop();
        for (auto dir : dirs) {
            int x = r + dir.first,  y = c + dir.second;
            // if x y out of range or it is obstasle, or has small distance aready
            if (x < 0 || y < 0 || x >= row || y >= col || rooms[x][y] <= rooms[r][c]+1) continue;
            rooms[x][y] = rooms[r][c] + 1;
            canReach.emplace(x, y);
        }
    }
}
```