---
layout: post
title:  "11. Container With Most Water"
date: 2019-01-26 10:41:23 -0400
categories: articles
---
Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

 

Example:
```
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```
# Function signature
```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        
    }
};
```
# 题意
就是如何让蓄水最大

# 想法
直接上了

# 参考答案
```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        if ( height.size() < 2) return 0;
        int res = 0, l = 0, r = height.size() - 1;
        while ( l < r ) {
            res = max(res, min(height[l], height[r])*(r-l));
            if ( height[l] < height[r] ) l++; // 注意这里是个坑，容易写错！！！
            else r--;
        }
        return res;
    }
};
```