---
layout: post
title:  "491. Increasing Subsequences"
date: 2019-01-21 15:46:23 -0400
categories: articles
---
Given an integer array, your task is to find all the different possible increasing subsequences of the given array, and the length of an increasing subsequence should be at least 2 .

Example:
```
Input: [4, 6, 7, 7]
Output: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]
```
Note:
The length of the given array will not exceed 15.
The range of integer in the given array is [-100,100].
The given array may contain duplicates, and two equal integers should also be considered as a special case of increasing sequence.
# Function signature
```c++
class Solution {
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        
    }
};
```
# 题意
给一个array，找到所有的subsequences，这些subsequences至少是2个元素组成。
# 想法
这是一道标准的DFS题。
# 尝试解解
```c++
// [-100,-100,0,0,0,100,100,0,0]
// 50/57
// Almost there!!!
class Solution {
public:
	void helper(vector<int>& nums, vector<int> temp, vector<vector<int>>& res, int idx){
		if ( temp.size() > 1) res.push_back(temp);
		for ( int i = idx; i < nums.size(); i++){
			if ( i > 0 && nums[i] == temp.back() && i - idx > 0 ) continue;
            if ( temp.back() <= nums[i]) {
                temp.push_back(nums[i]);
                helper( nums, temp , res, i + 1);
                temp.pop_back();
            }
		}
	}
    vector<vector<int>> findSubsequences(vector<int>& nums) {
    	vector<vector<int>> res;
    	if ( nums.size() == 0 ) return res;
        unordered_map<int, int> buff;
        for ( int i = 0; i < nums.size(); i++){
            if ( buff.find(nums[i]) == buff.end()) buff[nums[i]] = i;
        }
        for ( int i = 0; i < nums.size(); i++){
            if ( buff[nums[i]] < i) continue;
            helper(nums, vector<int>{nums[i]}, res, i + 1);
        }
    	return res;
    }
};
```
