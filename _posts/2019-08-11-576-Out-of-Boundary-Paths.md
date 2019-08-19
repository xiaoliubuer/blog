---
layout: post
title:  "576. Out of Boundary Paths"
date: 2019-08-11 17:35:00 -0400
categories: articles
---	
There is an m by n grid with a ball. Given the start coordinate (i,j) of the ball, you can move the ball to adjacent cell or cross the grid boundary in four directions (up, down, left, right). However, you can at most move N times. Find out the number of paths to move the ball out of grid boundary. The answer may be very large, return it after mod 109 + 7.

Example 1:
```
Input: m = 2, n = 2, N = 2, i = 0, j = 0
Output: 6
```
Explanation:

Example 2:
```
Input: m = 1, n = 3, N = 3, i = 0, j = 1
Output: 12
```
Explanation:

Note:
```
Once you move the ball out of boundary, you cannot move it back.
The length and height of the grid is in range [1,50].
N is in range [0,50].
```
```c++
class Solution {
public:
    vector< vector< vector<int> > > f;
    int dx[4] = {-1,0,1,0}, dy[4] = {0, 1, 0, -1};
    int mod = pow(10, 9) + 7;
    int dfs(int m, int n, int k, int x, int y){
        int &v = f[x][y][k];
        if(v != -1) return v; // calculate
        v = 0;
        if(k == 0) return v; // can't move
        
        for(int i = 0; i < 4; i++){
            int a = x + dx[i], b = y + dy[i];
            if( a < 0 || a == m || b < 0 || b == n) 
            	v++;
            else
                v += dfs(m, n , k - 1, a, b);
            v %= mod;
        }
        return v;
        
    }
    
    int findPaths(int m, int n, int N, int i, int j) {
        f = vector< vector< vector<int> > >(m, vector< vector<int> >(n, vector<int>(N+1, - 1)));
        return dfs(m , n, N, i, j);
        
    }
};
```
```c++
class Solution {
public:
    int findPaths(int m, int n, int N, int I, int J) {
        long long dp[51][51][51];
        memset(dp, 0, sizeof(dp));
        int mov[2][4] = { { 0, 0, -1, 1},{1, -1, 0, 0 } };
        long long ans = 0;
        int mod = 1e9 + 7;
        int x, y;
        dp[I][J][0] = 1;
        for(int move=0; move<N; move++){
            for(int i=0; i<m; i++){
                for(int j=0; j<n; j++){
                    if(dp[i][j][move] > 0){
                        for(int k=0; k<4; k++){
                            x = i + mov[0][k];
                            y = j + mov[1][k];
                            if(x<0 or y<0 or x>=m or y>=n){ 
                                ans += 1LL*dp[i][j][move];
                                ans = ans%mod;
                            }
                            else{ 
                                dp[x][y][move+1] += dp[i][j][move];
                                dp[x][y][move+1] = dp[x][y][move+1]%mod;
                            }
                        }
                    }
                }
            }
        }
        return ans%mod;
    }
};
```
```c++
class Solution {
    #define MOD 1000000007;
    long long ans = 0;
    vector<pair<int, int>> adj = { {0,1}, {0,-1}, {1,0}, {-1,0} };
    
    int findP(int m, int n, int N, int i, int j,
              vector<vector<vector<int>>>& dp)
    {
        if (N < 0)
            return 0;
        
        if(i<0 || i>=m || j<0 || j>=n)
        {
            return 1;
        }
        
        if (dp[i][j][N] != -1)
            return dp[i][j][N];
        
        int total = 0;
        for (auto p:adj)
        {
            int di = i+p.first;
            int dj = j+p.second;
            total += findP(m, n, N-1, di, dj, dp);
            total %= MOD;
        }
        return dp[i][j][N] = total;
    }
public:
    int findPaths(int m, int n, int N, int i, int j) {
        vector<vector<vector<int>>> dp(m, vector<vector<int>>(n, vector<int>(N+1, -1)));
        return findP(m, n, N, i, j, dp);
    }
};
```