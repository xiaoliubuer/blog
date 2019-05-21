---
layout: post
title:  "303. Range Sum Query - Immutable"
date: 2019-05-12 13:45:00 -0400
categories: articles
---
Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

Example:
```
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```
Note:
You may assume that the array does not change.
There are many calls to sumRange function.

```c++
// 2019-05-12
class NumArray {
public:
    NumArray(vector<int>& nums) {
        int curr = 0;
        for (int i : nums){
            curr += i;
            sum.push_back(curr);
        }
    }
    
    int sumRange(int i, int j) {
        return sum[j+1] - sum[i];
    }
    private:
    vector<int> sum = {0};
};
```