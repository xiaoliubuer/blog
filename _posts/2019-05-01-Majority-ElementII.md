---
layout: post
title:  "229. Majority Element II"
date: 2019-05-01 13:08:00 -0400
categories: articles
---

Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times.

Note: The algorithm should run in linear time and in O(1) space.

Example 1:
```
Input: [3,2,3]
Output: [3]
```
Example 2:
```
Input: [1,1,1,3,3,2,2,2]
Output: [1,2]
```
# Shame answer
```c++
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        int x=0, cx=0, y=0, cy=0;
        for (auto n: nums) {
            if (x == n) cx++;
            else if (y == n) cy++;
            else if (cx == 0) x=n, cx=1;
            else if (cy == 0) y=n, cy=1;
            else cx--, cy--;
        }
        cx = cy = 0;
        for (auto n: nums) {
            if (x == n) cx++;
            else if (y == n) cy++;
        }
        vector<int> ret;
        if (cx > nums.size()/3) ret.push_back(x);
        if (cy > nums.size()/3) ret.push_back(y);
        return ret;
    }
};
```