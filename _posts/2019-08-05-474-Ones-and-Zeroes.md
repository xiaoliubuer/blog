---
layout: post
title:  "474. Ones and Zeroes"
date: 2019-08-05 20:15:00 -0400
categories: articles
---
In the computer world, use restricted resource you have to generate maximum benefit is what we always want to pursue.

For now, suppose you are a dominator of m 0s and n 1s respectively. On the other hand, there is an array with strings consisting of only 0s and 1s.

Now your task is to find the maximum number of strings that you can form with given m 0s and n 1s. Each 0 and 1 can be used at most once.

Note:

The given numbers of 0s and 1s will both not exceed 100
The size of given string array won't exceed 600.
 

Example 1:
```
Input: Array = {"10", "0001", "111001", "1", "0"}, m = 5, n = 3
Output: 4
```
Explanation: This are totally 4 strings can be formed by the using of 5 0s and 3 1s, which are “10,”0001”,”1”,”0”
 

Example 2:
```
Input: Array = {"10", "0", "1"}, m = 1, n = 1
Output: 2
```
Explanation: You could form "10", but then you'd have nothing left. Better form "0" and "1".
```c++
class Solution {
 public:
  int findMaxForm(vector<string>& strs, int m, int n) {
    if (strs.size() == 0) {
      return 0;
    }
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    dp[0][0] = 0;
    for (const string str : strs) {
      int num_zeros = 0;
      int num_ones = 0;
      for (const char c : str) {
        if (c == '0') {
          num_zeros += 1;
        } else if (c == '1') {
          num_ones += 1;
        }
      }
      for (int i = m; i >= num_zeros; i -= 1) {
        for (int j = n; j >= num_ones; j -= 1) {
          dp[i][j] = max(dp[i][j], dp[i - num_zeros][j - num_ones] + 1);
        }
      }
    }
    return dp[m][n];
  }
};
```
```c++

class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m+1, vector<int>(n+1,0));
        for(string str:strs){
            int one=0;
            int zero=0;
            for(char s:str){
                if(s=='0') zero++;
                else if(s=='1') one++;
            }
            for(int i=m;i>=zero;i--){
                for(int j =n;j>=one;j--){
                    dp[i][j]=max(dp[i][j], dp[i-zero][j-one]+1);
                }
                
            }
        }
        return dp[m][n];
        
    }
};
```