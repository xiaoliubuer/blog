---
layout: post
title:  "322. Coin Change"
date: 2019-05-28 00:12:00 -0400
categories: articles
---
You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

Example 1:
```
Input: coins = [1, 2, 5], amount = 11
Output: 3 
Explanation: 11 = 5 + 5 + 1
```
Example 2:
```
Input: coins = [2], amount = 3
Output: -1
```
Note:
You may assume that you have an infinite number of each kind of coin.
```c++
// Easier understand answer
class Solution
{
public:
    int coinChange(vector<int>& coins, int amount) 
    {
        vector<int> dp(amount + 1, -1);
        dp[0] = 0;
        
        for (int i = 1; i <= amount; ++i)
            for (auto & c : coins)
                if (i - c >= 0 && dp[i - c] != -1)
                    dp[i] = dp[i] > 0 ? min(dp[i], dp[i - c] + 1) : dp[i - c] + 1;
        
        return dp[amount];
    }
};
```

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        if ( amount == 0 || coins.size() == 0 ) return 0;
        vector<int> dp(amount + 1, -1);
        dp[0] = 0;
        for(auto i:coins) {
            if ( i <= amount ) dp[i] = 1;
        }
        for (int i = 1; i <= amount; i++){
            int temp = INT_MAX;
            for (int j = 0; j < coins.size(); j++){
                if ( i - coins[j] >= 0){
                    temp = min(temp, dp[i - coins[j]]);
                }
            }
            dp[i] = temp == INT_MAX ? temp : temp + 1;
        }
        return dp[amount] == INT_MAX ? -1 : dp[amount];
    }
};
```