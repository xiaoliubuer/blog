---
layout: post
title:  "* Decode Ways"
date: 2019-02-06 21:53:23 -0400
categories: articles
---
A message containing letters from A-Z is being encoded to numbers using the following mapping:
```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```
Given a non-empty string containing only digits, determine the total number of ways to decode it.

Example 1:
```
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```
Example 2:
```
Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```
# Function signture
```c++
class Solution {
public:
    int numDecodings(string s) {
        
    }
};
```
# 题意

# 想法
# 尝试解解
```c++
class Solution {
public:
    int numDecodings(string s) {
        
    }
};
```

# Shame answer
```c++
class Solution {
public:
    int numDecodings(string s) {
        if(s.empty() || s[0]<'1' || s[0]>'9') return 0;
        vector<int> dp(s.size()+1,0);
        dp[0] = dp[1] = 1;
        
        for(int i = 1; i < s.size(); i++) {
            if( !isdigit(s[i]) ) return 0;
            int v = stoi(s.substr(i-1,2));
            if( v <= 26 && v > 9 ) dp[i+1] += dp[i-1];
            if( s[i] != '0' ) dp[i+1] += dp[i];
            if( dp[i+1] == 0 ) return 0;
        }
        return dp[s.size()];
    }
};
```