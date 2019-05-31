---
layout: post
title:  "96. Unique Binary Search Trees"
date: 2019-05-29 23:51:00 -0400
categories: articles
---
Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?

Example:
```
Input: 3
Output: 5
Explanation:
Given n = 3, there are a total of 5 unique BST's:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```
# Function signature
```c++
class Solution {
public:
    int numTrees(int n) {
        
    }
};
```
```c++
// 2019/05/29
// Best answer
class Solution {
public:
int numTrees(int n) {
    vector<int> dp(n+1, 0);
    dp[0] = 1;
    dp[1] = 1;
    for ( int i = 2; i <= n; i++ ) {
        for ( int j = 1; j <= i; j++ ) {
            dp[i] += dp[j - 1] * dp[ i - j ];
        }
    }
    return dp[n];
}

};
```