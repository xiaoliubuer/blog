---
layout: post
title:  "312. Burst Balloons"
date: 2019-07-30 08:32:00 -0400
categories: articles
---
Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] * nums[i] * nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

Note:

You may imagine nums[-1] = nums[n] = 1. They are not real therefore you can not burst them.
0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100
Example:

Input: [3,1,5,8]
Output: 167 
Explanation: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
             coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167

```c++
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        vector<int> num;
        num.push_back(1);
        num.insert(num.end(),nums.begin(),nums.end());
        num.push_back(1);
        
        int n = num.size();
        vector<vector<int>> dp(n+2,vector<int>(n,0));        
        
        for(int right=2;right<n;right++){
            for(int left=right-2;left>=0;left--){
                for(int i=left+1;i<right;i++){
                    dp[left][right] = max(dp[left][right],num[left]*num[right]*num[i] + dp[left][i] + dp[i][right]);
                }
            }
        }
        return dp[0][n-1];
    }
};
```
```c++
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int N=nums.size();
        nums.insert(nums.begin(),1);
        nums.insert(nums.end(),1);
        vector<vector<int>> max_coin(nums.size(),vector<int>(nums.size(),0));
        for(int len=1;len<=N;len++)
        {
            for(int start=1;start<=N-len+1;start++)
            {
                int end=start+len-1;
                int best_value=0;
                for(int last=start;last<=end;last++)
                {
                    int coins=max_coin[start][last-1]+max_coin[last+1][end];
                    coins+=nums[start-1]*nums[last]*nums[end+1];
                    best_value=max(best_value,coins);
                }
                max_coin[start][end]=best_value;
            }
        }
        return max_coin[1][N];
    }
};
```