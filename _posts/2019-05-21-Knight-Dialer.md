---
layout: post
title:  "935. Knight Dialer"
date: 2019-05-21 22:24:00 -0400
categories: articles
---
This time, we place our chess knight on any numbered key of a phone pad (indicated above), and the knight makes N-1 hops.  Each hop must be from one key to another numbered key.

Each time it lands on a key (including the initial placement of the knight), it presses the number of that key, pressing N digits total.

How many distinct numbers can you dial in this manner?

Since the answer may be large, output the answer modulo 10^9 + 7.
Example 1:
```
Input: 1
Output: 10
```
Example 2:
```
Input: 2
Output: 20
```
Example 3:
```
Input: 3
Output: 46
```
Note:
```
1 <= N <= 5000
```
```c++
class Solution {
public:
    int knightDialer(int N) {
        int mod = 1e9+7;
        vector<vector<int>> dp(10, vector<int>(2, 0));
        vector<vector<int>> reach = { {4, 6},{6, 8},{7, 9},{4, 8},{0, 3, 9}, {}, {0, 1, 7}, {2, 6}, {1, 3}, {2, 4} };
        
        for(int i = 1; i <= N; i++) {
            int cur = i%2;
            for(int j = 0; j < 10; j++) {
                if(i == 1) {dp[j][i] = 1; continue;}
                int sum = 0;
                for(auto & prev : reach[j]) sum = (sum + dp[prev][1-cur])%mod;
                dp[j][cur] = sum;
            }
        }
        
        int res = 0;
        for(int j = 0; j < 10; j++) res = (res+ dp[j][N%2])%mod;
        return res;
    }
};
```