---
layout: post
title:  "375. Guess Number Higher or Lower II"
date: 2018-12-29 11:54:23 -0400
categories: articles
---


# 参考答案
```c++
class Solution {
public:
    int getMoneyAmount(int n) {
        // 区间dp
        vector<vector<int>> dp(n + 1, vector<int> (n + 1, 0));
        
        for (int len = 1; len < n; ++len) {
            for (int i = 1; i + len <= n; ++i) {
                int j = i + len;
                dp[i][j] = j == i + 1 ? i : INT_MAX;
                for (int k = i + 1; k < j; ++k) {
                    dp[i][j] = min(dp[i][j], max(dp[i][k - 1], dp[k + 1][j]) + k);
                }
            }
        }
        
        return dp[1][n];
    }
};
```