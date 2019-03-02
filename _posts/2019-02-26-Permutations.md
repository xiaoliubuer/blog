---
layout: post
title:  "46. Permutations"
date: 2019-02-26 21:49:23 -0400
categories: articles
---
Given a collection of distinct integers, return all possible permutations.

Example:
```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```
# Function signature
```c++
class Solution {
public:
    void helper(vector<vector<int>>& res, vector<int> temp, vector<bool> selected, vector<int>& nums){
        if ( temp.size() == nums.size())
            res.push_back(temp);
        for (int i = 0; i < nums.size(); i++ ){
            if (selected[i] == false){
                temp.push_back(nums[i]);
                selected[i] = true;
                helper(res, temp, selected, nums);
                temp.pop_back();
                selected[i] = false;
            }
        }
    }
    
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        if (nums.size() == 0) return res;
        vector<int> temp;
        vector<bool> selected(nums.size(), false);
        helper(res, temp, selected, nums);
        return res;
        
    }
};
```