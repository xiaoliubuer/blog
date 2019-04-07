---
layout: post
title:  "77. Combinations"
date: 2019-04-02 07:48:23 -0400
categories: articles
---
Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

Example:
```
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```
```c++
class Solution {
public:
    void helper(vector<int>& nums, vector<vector<int>>& res, vector<int> temp, int idx, int k){
        if ( temp.size() == k ) {
            res.push_back(temp);
            return;
        }
        for (int i = idx; i < nums.size(); i++){
            temp.push_back(nums[i]);
            helper(nums, res, temp, i+1, k);
            temp.pop_back();
        }
    }
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> res;
        if ( n == 0 || k == 0 || n < k) return res;
        vector<int> nums;
        for (int i = 0; i < n; i++){
            nums.push_back(i+1);
        }
        vector<int> temp;
        helper(nums, res, temp, 0, k);
        return res;
    }
};
```