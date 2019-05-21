---
layout: post
title:  "* 41. First Missing Positive"
date: 2019-01-05 12:14:23 -0400
categories: articles
---
Given an unsorted integer array, find the smallest missing positive integer.

Example 1:
```
Input: [1,2,0]
Output: 3
```
Example 2:
```
Input: [3,4,-1,1]
Output: 2
```
Example 3:
```
Input: [7,8,9,11,12]
Output: 1
```
Note:
Your algorithm should run in O(n) time and uses constant extra space.
# Function signature
```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        
    }
};
```
# 题意
给一个未排序的int数组，找到最小的丢失的正整数。
# 想法
很有意思的一道题
保证是最小的miss的数字，那么这个数字会是什么呢？？最笨的办法就是用个set，把所有的数字都放进去。
```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
    	int res = 1;
    	for (int i = 0; i < nums.size(); i++){
    		if (nums[i] == res){
    			res++;
    		}
    	}
        return res;
    }
};
```

# Shame answer
```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        unordered_set<int> myset;
        for(auto i : nums) myset.emplace(i);
        int i = 1;
        while ( i < INT_MAX){
            if (myset.find(i) == myset.end()) return i;
            i++;
        }
        return -1;
    }
};
```
```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for(int i = 0; i < n; ++ i)
            while(nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i])
                swap(nums[i], nums[nums[i] - 1]);
        
        for(int i = 0; i < n; ++ i)
            if(nums[i] != i + 1)
                return i + 1;
        return n + 1;
    }
};
```