---
layout: post
title:  "70. Climbing Stairs"
date: 2019-02-11 10:06:23 -0400
categories: articles
---
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.

Example 1:
```
Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```
Example 2:
```
Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```
#Function signature
```c++
class Solution {
public:
    int helper(int n, vector<int>& visited){
        if ( visited[n] != 0 ) return visited[n];
        visited[n] = helper(n - 1, visited) + helper(n - 2, visited);
        return visited[n];
    }
    int climbStairs(int n) {
        if ( n < 4 ) return n;
        vector<int> visited(n + 1, 0);
        visited[1] = 1;
        visited[2] = 2;
    	helper(n, visited);
        return visited[n];
    }
};
```
# 题意
爬梯子，给一个n为梯子的阶数，可以一次爬1个，或一次爬2个，问有多少种爬的方法。

# 想法
DP