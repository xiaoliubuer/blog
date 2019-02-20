---
layout: post
title:  "** 72. Edit Distance"
date: 2019-01-29 21:21:23 -0400
categories: articles
---
Given two words word1 and word2, find the minimum number of operations required to convert word1 to word2.

You have the following 3 operations permitted on a word:

Insert a character
Delete a character
Replace a character
Example 1:
```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```
Example 2:
```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention ->  enention (replace 'i' with 'e')
enention ->  exention (replace 'n' with 'x')
exention ->  exection (replace 'n' with 'c')
exection ->  execution (insert 'u')
```
# Function signature
```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        
    }
};
```
# 题意
最小的转换从word1到word2。
# 想法
这种类型是一个类型的题。怎么转换呢？？
想法，看笔记！！！
# 尝试解解
```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.length(), n = word2.length();
        if ( m == 0 ) return n;
        if ( n == 0 ) return m;
        int dp[m][n] = {0};
        dp[0][0] = word1[0] == word2[0] ? 0 : 1;
        for (int i = 1; i < m; ++i){
            if ( word1[i] == word2[0]) dp[i][0] = i;
            else dp[i][0] = dp[i-1][0] + 1;
        }
        for (int i = 1; i < n; ++i){
            if ( word2[i] == word1[0]) dp[0][i] = i;
            else dp[0][i] = dp[0][i-1] + 1;
        }
        for (int i = 1; i < m; ++i){
            for (int j = 1; j < n; ++j){
                int len = min(dp[i-1][j-1], min(dp[i-1][j], dp[i][j-1]));
                dp[i][j] = word1[i] == word2[j] ?  len : len + 1;
            }
        }
        return dp[m-1][n-1];
    }
};
```
# Shame answer
```java
public int minDistance(String s1, String s2) {
        // DP table creation
	//用一个二维数组
        int[][] dpt = new int[s1.length() + 1][s2.length() + 1];
        
        // DP table initialization
//把最后一竖列和最后一横列赋值为

        for (int i = 0; i < dpt.length; i++) {
            dpt[i][s2.length()] = s1.length() - i;
        }
        
        for (int j = 0; j < dpt[0].length; j++) {
            dpt[s1.length()][j] = s2.length() - j;
        }
        
		// Actual calculation of the minimum number of operations
        for (int i = s1.length() - 1; i >= 0; i--) {
            for (int j = s2.length() - 1; j >= 0; j--) {
                if (s1.charAt(i) == s2.charAt(j))
                    dpt[i][j] = dpt[i+1][j+1];
                else
                    dpt[i][j] = 1 + Math.min(dpt[i+1][j], Math.min(dpt[i][j+1], dpt[i+1][j+1]));
            }
        }        
        return dpt[0][0];
    }
```
