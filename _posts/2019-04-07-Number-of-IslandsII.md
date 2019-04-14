---
layout: post
title:  "305. Number of Islands II"
date: 2019-04-07 18:51:00 -0400
categories: articles
---
A 2d grid map of m rows and n columns is initially filled with water. We may perform an addLand operation which turns the water at position (row, col) into a land. Given a list of positions to operate, count the number of islands after each addLand operation. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example:
```
Input: m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]]
Output: [1,1,2,3]
```
Explanation:

Initially, the 2d grid grid is filled with water. (Assume 0 represents water and 1 represents land).
```
0 0 0
0 0 0
0 0 0
```
Operation #1: addLand(0, 0) turns the water at grid[0][0] into a land.
```
1 0 0
0 0 0   Number of islands = 1
0 0 0
```
Operation #2: addLand(0, 1) turns the water at grid[0][1] into a land.
```
1 1 0
0 0 0   Number of islands = 1
0 0 0
```
Operation #3: addLand(1, 2) turns the water at grid[1][2] into a land.
```
1 1 0
0 0 1   Number of islands = 2
0 0 0
```
Operation #4: addLand(2, 1) turns the water at grid[2][1] into a land.
```
1 1 0
0 0 1   Number of islands = 3
0 1 0
```
Follow up:

Can you do it in time complexity O(k log mn), where k is the length of the positions?

```c++
class Solution {
public:
    int Find(int val, vector<int>& parent){
        while( val != parent[val]){
            parent[val] = parent[parent[val]];
            val = parent[val];
        }
        return val;
    }
    
    void Union(int p1, int p2, vector<int>& parent){
        parent[p2] = p1;
    }
    
    int dir[4][2] = { {1, 0}, {-1, 0}, {0, 1}, {0, -1} };
    vector<int> numIslands2(int m, int n, vector<pair<int, int>>& positions) {
        
        vector<int> res;
        vector<int> parent(m*n, -1);
        if ( m <= 0 || n <= 0 ) return res;
        int count = 0;
        for (auto i : positions){
            int root = n * i.first + i.second;
            parent[root] = root;
            count++;
            for (auto d : dir) {
                int x = i.first + d[0];
                int y = i.second+ d[1];
                int nb = x * n + y;
                if ( x < 0 || x >= m || y < 0 || y >= n || parent[nb] == -1 ) continue;
                int nbroot = Find(nb, parent);
                if (root != nbroot){
                    Union(root, nbroot, parent);
                    count--;
                }
            }
            res.push_back(count);
        }
        return res;
    }
};
```