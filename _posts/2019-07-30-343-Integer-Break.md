---
layout: post
title:  "343. Integer Break"
date: 2019-07-30 20:13:00 -0400
categories: articles
---
Given a positive integer n, break it into the sum of at least two positive integers and maximize the product of those integers. Return the maximum product you can get.

Example 1:
```
Input: 2
Output: 1
Explanation: 2 = 1 + 1, 1 × 1 = 1.
```
Example 2:
```
Input: 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
```
Note: You may assume that n is not less than 2 and not larger than 58.

```c++

class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n+1,0);
        
        dp[2] = 1;
        if (n == 2) return 1;
        
        dp[3] = 2;
        if (n == 3) return 2;
        
        for (int i=4; i<=n; ++i) {
            for (int j=1; j<i-1; ++j) {
                dp[i] = max(dp[i], j*(i-j));
                dp[i] = max(dp[i], j*dp[i-j]);
            }
        }
        
        return dp[n];
    }
};
```
```c++
class Solution {
public:
int integerBreak(int n) {
    vector<int> dp(n+2, 0);
    dp[2] = 2;
    dp[3] = 3;
    if(n <= 3) return dp[n]-1;
    for(int i = 4; i < n+1; i++){
        for(int j = 1; j < i; j++)
            dp[i] = max(dp[i], dp[j]*(i-j)); 
    }            
    return dp[n];
}
};
```