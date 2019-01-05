---
layout: post
title:  "* 42. Trapping Rain Water"
date: 2019-01-05 14:13:23 -0400
categories: articles
---
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped. Thanks Marcos for contributing this image!

Example:

Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6

# Function signature
```c++
class Solution {
public:
    int trap(vector<int>& height) {
        
    }
};
```
# 题意
给了一个array代表沟沟槽槽的高度，问下了一场雨后能储多少水。
# 想法
这是一道什么题呢??

# Shame answer
```c++
class Solution {
public:
    int trap(vector<int>& v) {
        int ans = 0;
        vector<int> lm(v.size(),0);
        vector<int> rm(v.size(),0);
        for(int i = 1; i < v.size(); i++) lm[i] = max(lm[i-1], v[i-1]);
        for(int i = v.size()-2; i >=0 ; i--) rm[i] = max(rm[i+1], v[i+1]);
        for(int i = 0; i < v.size(); i++)
            ans += max(0, min(lm[i],rm[i]) - v[i]);
        return ans;
    }
};
```