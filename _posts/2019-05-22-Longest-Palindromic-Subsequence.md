---
layout: post
title:  "516. Longest Palindromic Subsequence"
date: 2019-05-22 21:04:00 -0400
categories: articles
---

Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.

Example 1:
```
Input:

"bbbab"
Output:
4
One possible longest palindromic subsequence is "bbbb".
```
Example 2:
```
Input:

"cbbd"
Output:
2
One possible longest palindromic subsequence is "bb".
```
```c++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size(), res = 0;
        vector<int> dp(n, 1);
        for (int i = 1; i < n; i++) {
            int len = 0;
            for (int j = i - 1; j >= 0; j--) {
                int tmp = dp[j];
                if (s[j] == s[i]) {
                    dp[j] = len + 2;
                }
                len = max(len, tmp);
            }
        }
        for (auto i : dp)
            res = max(res, i);
        return res;
    }
};
```