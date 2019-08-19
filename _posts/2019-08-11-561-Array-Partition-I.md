---
layout: post
title:  "561. Array Partition I"
date: 2019-08-11 16:52:00 -0400
categories: articles
---
Given an array of 2n integers, your task is to group these integers into n pairs of integer, say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible.

Example 1:
```
Input: [1,4,3,2]

Output: 4
Explanation: n is 2, and the maximum sum of pairs is 4 = min(1, 2) + min(3, 4).
```
Note:
```
n is a positive integer, which is in the range of [1, 10000].
All the integers in the array will be in the range of [-10000, 10000].
```
```c++
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        int res = 0;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i+=2){
            res += nums[i];
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int sum = 0;
        for (int i = 0; i < nums.size(); i+=2) {
            sum += nums[i];
        }
        return sum;
    }
};
```
```c++
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int result = 0;
        for (unsigned long int i = 0; i < nums.size(); ++i) {
            if (i % 2 == 0) result += nums[i];
        }
        return result;
    }
};
```
```c++
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int sum = 0;
        for (int i=0; i < nums.size(); i+=2) {
            sum += nums[i];
        }
        return sum;
    }
};
```