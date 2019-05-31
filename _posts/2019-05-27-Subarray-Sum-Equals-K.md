---
layout: post
title:  "560. Subarray Sum Equals K"
date: 2019-05-27 19:54:00 -0400
categories: articles
---

Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.

Example 1:
```
Input:nums = [1,1,1], k = 2
Output: 2
```
Note:
```
The length of the array is in range [1, 20,000].
The range of numbers in the array is [-1000, 1000] and the range of the integer k is [-1e7, 1e7].
```
```c++
// Accepted! 2019-05-27
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> freq;
        int sum = 0, count = 0;
        freq[0] = 1;
        
        for ( int i = 0; i < nums.size(); i++ ) {
            sum += nums[i];
            count += freq[ sum - k ];
            freq[sum]++;
        }
        return count;
    }
};
```