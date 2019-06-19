---
layout: post
title:  "283. Move Zeroes"
date: 2019-06-18 19:15:00 -0400
categories: articles
---
Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Example:
```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
Note:
```
You must do this in-place without making a copy of the array.
Minimize the total number of operations.
```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        if ( nums.size() == 0 ) return;
        for( int fast = 0, slow = 0; fast < nums.size(); fast++ ){
            if (nums[fast] != 0){
                swap(nums[slow], nums[fast]);
                slow++;
        }
    }
    }
};
```