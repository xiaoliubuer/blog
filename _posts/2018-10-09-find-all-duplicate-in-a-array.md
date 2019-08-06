---
layout: post
title:  "442. Find All Duplicates in a Array"
date: 2018-10-09 20:19:23 -0400
categories: articles
---
Given an array of integers, 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements that appear twice in this array.

Could you do it without extra space and in O(n) runtime?

Example:
```
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]
```
思路:

这道题和287非常类似！！！但就是tm过不了。蛋疼！！！
```c++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        unordered_set<int> check;
        vector<int> res;
        for (auto i:nums){
            if (check.find(i) != check.end())
                res.push_back(i);
            else
                check.insert(i);
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> res;
        for(int i=0;i<nums.size();++i){
            int idx=nums[i] - 1;
            if(nums[idx] != nums[i]) {
                swap(nums[idx],nums[i]);
                --i;
            }
        }
        for(int i=0;i<nums.size();++i){
            if(nums[i]!=i+1) res.push_back(nums[i]);
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> res;
        int n=nums.size();
        for(int i=0;i<nums.size();++i){
            int idx=nums[i]-1;
            nums[idx%n] += n;
        }
        for(int i=0;i<nums.size();++i){
            if(nums[i]>2*n) res.push_back(i+1);
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> res;
        for (int i = 0; i < nums.size(); ++i){
            int idx = abs(nums[i]) - 1;
            if (nums[idx] <0) res.push_back(idx+1);
            nums[idx] = -nums[idx];
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> res;
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] != nums[nums[i] - 1]) {
                swap(nums[i], nums[nums[i] - 1]);
                --i;
            }
        }
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] != i + 1) res.push_back(nums[i]);
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> res;
        for (int i = 0; i < nums.size(); i++) {
            int tmp = abs(nums[i]) - 1;
            nums[tmp] = -nums[tmp];
            if (nums[tmp] > 0) res.push_back(tmp + 1);
        }
        return res;
    }
};
```