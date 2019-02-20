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
给一个n，有多少种BST的结构