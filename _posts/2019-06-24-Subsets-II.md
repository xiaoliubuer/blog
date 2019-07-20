---
layout: post
title:  "90. Subsets II"
date: 2019-06-24 19:28:00 -0400
categories: articles
---
Given a collection of integers that might contain __duplicates__, nums, return all possible subsets (the power set).

Note: The solution set must not contain duplicate subsets.

Example:
```
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```
```c++
class Solution {
public:
    
    void helper(vector<vector<int>>& res, vector<int> temp, int idx, vector<int>& nums){
        res.push_back(temp);
        for (int i = idx; i < nums.size(); i++){
            if ( nums[i-1] == nums[i] && i > idx) continue;
            temp.push_back(nums[i]);
            helper(res, temp, i+1, nums);
            temp.pop_back();
        }
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<vector<int>> res;
        if ( nums.size() == 0 ) return res;
        vector<int> temp;
        sort(nums.begin(), nums.end());
        helper(res, temp, 0, nums);
        return res;
    }
};
```