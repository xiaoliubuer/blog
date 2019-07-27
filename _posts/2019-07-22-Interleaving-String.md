---
layout: post
title:  "97. Interleaving String"
date: 2019-07-22 21:49:00 -0400
categories: articles
---
Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.

Example 1:
```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
```
Example 2:
```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
Output: false
```

```c++
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        if (s1.size() + s2.size() != s3.size()) return false;
        
        if (s1.size() == 0) return s2 == s3;
        if (s2.size() == 0) return s1 == s3;
        
        int m = s1.size();
        int n = s2.size();
        
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0)); 
        
        dp[0][0] = 1;
        
        for (int i = 1; i <= m; i++) {
            dp[i][0] = s1[i - 1] == s3[i - 1] && dp[i - 1][0] == 1;
        }
        
        for (int j = 1; j <= n; j++) {
            dp[0][j] = s2[j - 1] == s3[j - 1] && dp[0][j - 1] == 1;
        }
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                dp[i][j] = dp[i - 1][j] && s1[i - 1] == s3[i + j - 1] || dp[i][j - 1] && s2[j - 1] == s3[i + j - 1];
            }
        }
        
        return dp[m][n] == 1;
    }
};
```