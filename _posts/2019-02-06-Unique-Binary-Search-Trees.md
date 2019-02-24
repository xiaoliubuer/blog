---
layout: post
title:  "96. Unique Binary Search Trees"
date: 2019-02-06 21:56:23 -0400
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
# 题意
给一个n，有多少种BST的结构。
# 想法
很有意思的一道题。
# Shame answer
```c++
class Solution {
public:
int numTrees(int n) 
{
    vector<int> t(n + 1, 0);
    t[0] = t[1] = 1;
    int i, j;
    for (i = 2; i <= n; ++i)
    {
        for (j = 1; j <= i / 2; ++j)
            t[i] += t[j - 1] * t[i - j];
        t[i] *= 2;
        if (i % 2)
            t[i] += t[i / 2] * t[i / 2];//Plus the middle 'root' trees.
    }
    return t[n];
    }
};
```

```c++
public int numTrees(int n) {
    int [] G = new int[n+1];
    G[0] = G[1] = 1;
    
    for(int i=2; i<=n; ++i) {
    	for(int j=1; j<=i; ++j) {
    		G[i] += G[j-1] * G[i-j];
    	}
    }

    return G[n];
}
```