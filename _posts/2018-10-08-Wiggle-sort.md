---
layout: post
title:  "280. Wiggle Sort"
date: 2018-10-08 22:12:23 -0400
categories: articles
---

Given an unsorted array nums, reorder it in-place such that nums[0] <= nums[1] >= nums[2] <= nums[3]....

Example:

Input: nums = [3,5,2,1,6,4]
Output: One possible answer is [3,5,1,6,2,4]

思路:
一定要把这个array变成严格 /\/\/\ 的样子。
所以，这个就很有意思啦。
在even位，保证小于i-1,在odd位，保证大于i-1,否则交换。
```c++
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        if (nums.size() < 2) return;
        for (int i = 1; i < nums.size(); i++){
            if (i%2 == 0 && nums[i] > nums[i-1])
                swap(nums[i], nums[i-1]);
            if (i%2 == 1 && nums[i] < nums[i-1])
                swap(nums[i], nums[i-1]);
        }
    }
};
```