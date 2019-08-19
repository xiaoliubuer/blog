---
layout: post
title:  "562. Longest Line of Consecutive One in Matrix"
date: 2019-08-11 16:55:00 -0400
categories: articles
---
Given a 01 matrix M, find the longest line of consecutive one in the matrix. The line could be horizontal, vertical, diagonal or anti-diagonal.
Example:
Input:
```
[[0,1,1,0],
 [0,1,1,0],
 [0,0,0,1]]
Output: 3
```
Hint: The number of elements in the given matrix will not exceed 10,000.

```c++
class Solution {
private:
    int move[4][2] = { {0, 1}, {1, 0}, {1, 1}, {1, -1} };
public:
    int longestLine(vector<vector<int>>& M) {
        int res = 0;
        if (M.empty() || M[0].empty())
            return 0;
        int m = M.size(), n = M[0].size();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (M[i][j] == 1) {
                    for (int k = 0; k < 4; k++) {
                        int preX = i - move[k][0];
                        int preY = j - move[k][1];
                        if (preX >= 0 && preX < m && preY >= 0 && preY < n && M[preX][preY] == 1)
                            continue;
                        int x = i + move[k][0];
                        int y = j + move[k][1];
                        int count = 1;
                        while(x >= 0 && x < m && y >= 0 && y < n && M[x][y] == 1) {
                            count++;
                            x = x + move[k][0];
                            y = y + move[k][1];
                        }
                        res = max(count, res);
                    }
                }
            }
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int longestLine(vector<vector<int>>& grid) {
        if (grid.size() == 0) return 0;
        
        vector<vector<tuple<int, int, int, int>>> dp(grid.size() + 1, vector<tuple<int, int, int, int>>(grid[0].size() + 1));
        
        int result = 0;
        
        for (int i = 0; i < grid.size(); i++) {
            for (int j = 0; j < grid[i].size(); j++) {
                if (grid[i][j] == 0) continue;
                auto& val = dp[i+1][j+1];
                
                get<0>(val) = get<0>(dp[i][j+1]) + 1;
                get<1>(val) = get<1>(dp[i+1][j]) + 1;
                get<2>(val) = get<2>(dp[i][j]) + 1;                
                result = max(result, max(get<0>(val), max(get<1>(val), get<2>(val))));
            }
        }
        
        for (int i = 0; i < grid.size(); i++) {
            for (int j = grid[i].size() - 1; j >= 0; j--) {
                if (grid[i][j] == 0) continue;
                auto& val = dp[i+1][j];
                
                get<3>(val) = get<3>(dp[i][j+1]) + 1;   
                result = max(result, get<3>(val));
            }
        }
        
        return result;
    }
};
```