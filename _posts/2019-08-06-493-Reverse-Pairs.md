---
layout: post
title:  "493. Reverse Pairs"
date: 2019-08-06 21:03:00 -0400
categories: articles
---
Given an array nums, we call (i, j) an important reverse pair if i < j and nums[i] > 2 * nums[j].
You need to return the number of important reverse pairs in the given array.
Example1:
```
Input: [1,3,2,3,1]
Output: 2
```
Example2:
```
Input: [2,4,3,5,1]
Output: 3
```
Note:
```
The length of the given array will not exceed 50,000.
All the numbers in the input array are in the range of 32-bit integer.
```
```c++
class Solution {
    vector<int> temp;
public:
    int reversePairs(vector<int>& nums) {
        temp.resize(nums.size());
        int res = merge_count(nums, 0, nums.size()-1);
        return res;
    }
    
    int merge_count(vector<int> &nums, int begin, int end) {
        if(begin >= end)
            return 0;
        int mid = begin+(end-begin)/2;
        int left = merge_count(nums, begin, mid);
        int right = merge_count(nums, mid+1, end);
        return left+right+count_mid(nums, begin, end, mid);
    }
    
    int count_mid(vector<int> & nums, int begin, int end, int mid){
        int l = begin;
        int r = mid+1;
        int cur = begin;
        int num = 0;
        int last_twice = mid+1;
        while(l<=mid) {
            while(last_twice<=end&&nums[l]>nums[last_twice]*2l){
                num+=mid-l+1;
                last_twice++;
            }
            while(r<=end&&nums[l]>nums[r]) {
                temp[cur++] = nums[r++]; 
            }
            temp[cur++] = nums[l++];
        }
        while(r<=end)
            temp[cur++]=nums[r++];
        for(int i=begin; i<=end; i++)
            nums[i] = temp[i];
        return num;
    }
};
```