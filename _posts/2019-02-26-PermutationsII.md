---
layout: post
title:  "47. Permutations II"
date: 2019-02-26 21:52:23 -0400
categories: articles
---
Given a collection of numbers that might contain duplicates, return all possible unique permutations.

Example:
```
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```
# Function signature
```c++
class Solution {
public:
    void helper(vector<int>& nums, vector<int> temp, vector<vector<int>>& res, vector<bool> visited){
        if (nums.size() == temp.size()){
            res.push_back(temp);
            return;
        }
        
        for (int i = 0; i < nums.size(); i++){
            if ( visited[i] || (i > 0 && nums[i] == nums[i-1] && !visited[i-1])){  
                continue;
            }
            else{
                vector<int> t = temp;
                t.push_back(nums[i]);
                visited[i] = true;
                helper(nums, t, res, visited);
                visited[i] = false;
            }
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> res;
        if ( nums.size() == 0 ) res;
        vector<int> temp;
        sort(nums.begin(), nums.end());
        vector<bool> visited(nums.size(), false);
        helper(nums, temp, res, visited);
        return res;
    }
};
```
```c++
class Solution {
public:
    void helper(vector<int>& nums, vector<vector<int>>& res, vector<int> temp, vector<bool> visited){
        if ( nums.size() == temp.size()) res.push_back(temp);
        
        for (int i = 0; i < nums.size(); i++){
            if (visited[i] == true || (i > 0 && nums[i] == nums[i-1] && visited[i-1] == false)) continue;// visited[i-1] == false 这里要想清楚！！！！
            else{
                temp.push_back(nums[i]);
                visited[i] = true;
                helper(nums, res, temp, visited);
                visited[i] = false;
                temp.pop_back();
            }
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        vector<bool> visited(nums.size(), false);
        vector<int> temp;
        helper(nums, res, temp, visited);
        return res;
    }
};
```