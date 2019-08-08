---
layout: post
title:  "523. Continuous Subarray Sum"
date: 2019-08-07 20:30:00 -0400
categories: articles
---

Given a list of non-negative numbers and a target integer k, write a function to check if the array has a continuous subarray of size at least 2 that sums up to a multiple of k, that is, sums up to n * k where n is also an integer.

Example 1:
```
Input: [23, 2, 4, 6, 7],  k=6
Output: True
Explanation: Because [2, 4] is a continuous subarray of size 2 and sums up to 6.
```
Example 2:
```
Input: [23, 2, 6, 4, 7],  k=6
Output: True
Explanation: Because [23, 2, 6, 4, 7] is an continuous subarray of size 5 and sums up to 42.
```

Note:
```
The length of the array won't exceed 10,000.
You may assume the sum of all the numbers is in the range of a signed 32-bit integer.
```
```c++
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        unordered_map<int,int> sum_to_index;
        int runningSum = 0;
        for(int i = 0; i< nums.size(); i++) {
            runningSum+=nums[i];
            if(k!=0)
               runningSum = runningSum % k;
            if(runningSum == 0 && i>=1)
                return true;
            if(sum_to_index.find(runningSum) != sum_to_index.end()) {
                if( i - sum_to_index[runningSum] >1)
                    return true;
                else
                   continue;
            }
            sum_to_index[runningSum] = i;
            //sum_to_index.insert({runningSum, i}); 
        }
        return false;        
    }
};
```
```c++
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        unordered_map<int,int> sum_to_index;
        int runningSum = 0;
        for(int i = 0; i< nums.size(); i++) {
            runningSum+=nums[i];
            if(k!=0)
               runningSum = runningSum % k;
            if(runningSum == 0 && i>=1)
                return true;
            if(sum_to_index.find(runningSum) != sum_to_index.end() && i - sum_to_index[runningSum] >1 )
                return true;
            sum_to_index.insert({runningSum, i});
        }
        return false;        
    }
};
```