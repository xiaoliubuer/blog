---
layout: post
title:  "279. Perfect Squares"
date: 2019-03-29 19:12:23 -0400
categories: articles
---
Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.

Example 1:
```
Input: n = 12
Output: 3 
Explanation: 12 = 4 + 4 + 4.
```
Example 2:
```
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

```c++
//Timeout
class Solution {
public:
    int res = 1000;
    void helper(int n, int count) {
        if (n == 0) res = min(res, count);
        int root = sqrt(n);
        while( root > 0){
            helper( n - root*root, count+1);
            root--;
        }
    }
    int numSquares(int n) {
        if (n == 0) return 0;
        helper(n, 0);
        return res;
    }
};
```

```c++
// Accepted!!!
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n + 1, 10);
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; i++ ){
            int root = sqrt(i);
            while ( root > 0 ){
                dp[i] = min(dp[i - root*root] + 1, dp[i]);
                root--;
            }
        }
        return dp[n];
    }
};
```
```c++
//Accepted!!
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n+1, 100);
        dp[0] = 0;
        dp[1] = 1;
        
        for(int i = 2; i <= n; i++) {
            int root = 1;
            while (root*root <= i){
                dp[i] = min( dp[i - root*root] + 1, dp[i]);
                root++;
            }
        }
        return dp[n];
    }
};
```