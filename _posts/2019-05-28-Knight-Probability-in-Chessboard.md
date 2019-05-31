---
layout: post
title:  "688. Knight Probability in Chessboard"
date: 2019-05-28 17:45:00 -0400
categories: articles
---
On an NxN chessboard, a knight starts at the r-th row and c-th column and attempts to make exactly K moves. The rows and columns are 0 indexed, so the top-left square is (0, 0), and the bottom-right square is (N-1, N-1).

A chess knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.

Each time the knight is to move, it chooses one of eight possible moves uniformly at random (even if the piece would go off the chessboard) and moves there.

The knight continues moving until it has made exactly K moves or has moved off the chessboard. Return the probability that the knight remains on the board after it has stopped moving.

Example:
```
Input: 3, 2, 0, 0
Output: 0.0625
Explanation: There are two moves (to (1,2), (2,1)) that will keep the knight on the board.
From each of those positions, there are also two moves that will keep the knight on the board.
The total probability the knight stays on the board is 0.0625.
```

Note:
```
N will be between 1 and 25.
K will be between 0 and 100.
The knight always initially starts on the board.
```
```c++
// Faster answer
class Solution {
public:
    double knightProbability(int N, int K, int r, int c) {
        vector<vector<double>> dp(N, vector<double>(N, 0));
        int dr[] = {2, 2, 1, 1, -1, -1, -2, -2};
        int dc[] = {1, -1, 2, -2, 2, -2, 1, -1};
        
        dp[r][c] = 1;
        for (; K > 0 ; K--) {
            vector<vector<double>> dp2(N, vector<double>(N, 0));
            for ( int r = 0; r < N; r++ ) {
                for ( int c = 0; c < N; c++ ) {
                    for ( int k = 0; k < 8; k++ ) {
                        int cr = r + dr[k];
                        int cc = c + dc[k];
                        if ( 0 <= cr && cr < N && 0 <= cc && cc < N) {
                            dp2[cr][cc] += dp[r][c] / 8.0;
                        }
                    }
                }
            }
            dp = dp2;
        }
        double ans = 0.0;
        for ( auto row : dp ) {
            for ( double x : row ) ans += x;
        }
        return ans;
    }
};
```

```c++
class Solution {
private:
    unordered_map<int, unordered_map<int, unordered_map<int, double>>>dp;
public:
    double knightProbability(int N, int K, int r, int c) {
        if(dp.count(r) && dp[r].count(c) && dp[r][c].count(K)) return dp[r][c][K];
        if(r < 0 || r >= N || c < 0 || c >= N) return 0;
        if(K == 0) return 1;
        double total = knightProbability(N, K - 1, r - 1, c - 2) + knightProbability(N, K - 1, r - 2, c - 1) 
                     + knightProbability(N, K - 1, r - 1, c + 2) + knightProbability(N, K - 1, r - 2, c + 1) 
                     + knightProbability(N, K - 1, r + 1, c + 2) + knightProbability(N, K - 1, r + 2, c + 1) 
                     + knightProbability(N, K - 1, r + 1, c - 2) + knightProbability(N, K - 1, r + 2, c - 1);
        double res = total / 8;
        dp[r][c][K] = res;
        return res;
    }
};
```