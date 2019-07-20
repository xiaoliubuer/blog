---
layout: post
title:  "132. Palindrome Partitioning II"
date: 2019-06-25 21:17:00 -0400
categories: articles
---
Given a string s, partition s such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of s.

Example:
```
Input: "aab"
Output: 1
Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
```
```c++
class Solution {
public:
   int minCut(string s) {
        int n =s.size();
        vector<int> dp(n+1,0);
         for (int i = 0; i <=n; ++i) dp[i] = n -i -1;
         vector<vector<bool>> m (n, vector<bool>(n, false));
         for (int i = n - 1; i >= 0; --i) {
            for (int j = i; j < n; ++j) {
                if (s[i] == s[j] && (j - i <= 1 || m[i + 1][j - 1])) {
                    m[i][j] = true;
                    dp[i] = min(dp[i], dp[j + 1] + 1); 
                }
            }
        }
       return dp[0]; 
    }
};
```

```c++
class Solution {
public:
    int minCut(string s) {
        auto f = vector<int>(s.size(), INT_MAX);
        for (int i = 0; i < s.size(); ++i) {
            // odd
            for (int len = 0; 
                     i - len >= 0 && 
                     i + len < s.size() && 
                     s[i-len] == s[i+len]; ++len) {
                f[i+len] = min(f[i+len], i-len == 0 ? 0 : f[i-len-1] + 1);
            }
            for (int len = 1; i + 1 -len >= 0 &&
                              i + len < s.size() &&
                              s[i+1-len] == s[i+len]; ++len) {
                f[i+len] = min(f[i+len], i+1-len==0?0: f[i+1-len-1]+1);
            }
        }
        return f.back();
    }
};
```