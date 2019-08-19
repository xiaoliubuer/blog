---
layout: post
title:  "583. Delete Operation for Two Strings"
date: 2019-08-11 17:48:00 -0400
categories: articles
---	
Given two words word1 and word2, find the minimum number of steps required to make word1 and word2 the same, where in each step you can delete one character in either string.

Example 1:
```
Input: "sea", "eat"
Output: 2
```
Explanation: 
```
You need one step to make "sea" to "ea" and another step to make "eat" to "ea".
```
Note:
```
The length of given words won't exceed 500.
Characters in given words can only be lower-case letters.
```
```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int size1 = word1.size(), size2 = word2.size();
        vector<int> dp(size2+1, 0), other(size2+1, 0);
        for(int i = 0; i < size1; ++i) {
            for(int j = 1; j <= size2; ++j) {
                if(word1[i] == word2[j-1]) dp[j] = other[j-1] + 1;
                else dp[j] = max(other[j], dp[j-1]);
            }
            swap(dp, other);
        }

        return size1 + size2 - 2 * other[size2];
    }
};
```
```c++
class Solution {
public:
    int minDistance(string a, string b) {
        vector<vector<int>> A(a.size()+1,vector<int>(b.size()+1));
        for(int i=1;i<=a.size();i++){
            for(int j=1;j<=b.size();j++){
                if(a[i-1]==b[j-1]){
                    A[i][j]=A[i-1][j-1]+1;
                }
                else{
                    A[i][j]=max(A[i-1][j],A[i][j-1]);
                }
            }
        }
        int len=A[a.size()][b.size()];
        return (a.size()+b.size()-2*len);
    }
};
```
```c++
class Solution {
public:
    int minDistance(string str1, string str2) {
        int n1 = str1.size();
        int n2 = str2.size();
        
        vector<vector<int>> dp(n1+1, vector<int>(n2+1, -1));
        
        for (int i = 0; i <= n1; ++i) {
            dp[i][0] = 0;
        }
        for (int i = 0; i <= n2; ++i) {
            dp[0][i] = 0;
        }
        
        for (int i = 0; i < n1; ++i) {
            for (int j = 0; j < n2; ++j) {
                if (str1[i] == str2[j]) {
                    dp[i+1][j+1] = dp[i][j] + 1;
                } else {
                    dp[i+1][j+1] = max(dp[i+1][j], dp[i][j+1]);
                }
            }
        }
        
        return (n1+n2 - 2*dp[n1][n2]);
    }
};
```