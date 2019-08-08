---
layout: post
title:  "491. Increasing Subsequences"
date: 2019-08-06 20:39:00 -0400
categories: articles
---
Given an integer array, your task is to find all the different possible increasing subsequences of the given array, and the length of an increasing subsequence should be at least 2.

Example:
```
Input: [4, 6, 7, 7]
Output: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]
```
Note:
```
The length of the given array will not exceed 15.
The range of integer in the given array is [-100,100].
The given array may contain duplicates, and two equal integers should also be considered as a special case of increasing sequence.
```
```c++
class Solution {
public:
    void dfs(int start, vector<int>& nums, vector<int>& res, vector<vector<int>>& ans){
        if(res.size() >1){
            ans.push_back(res);
        }
        unordered_set<int> vis;
        for(int i=start; i<nums.size(); i++){
            if(res.size() && nums[i] < res.back()) continue;
            if(vis.count(nums[i])) continue;
            vis.insert(nums[i]);
            res.push_back(nums[i]);
            dfs(i+1, nums, res, ans);
            res.pop_back();
        }
    }
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        vector<int> res;
        vector<vector<int>> ans;
        dfs(0, nums, res, ans);
        return ans;
    }
};
```
```c++
class Solution {
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        if(nums.empty()) return {};
        vector<vector<int>> sol;
        vector<int> cur;
        permute(sol, nums, cur, 0);
        return sol;
    }
    static void permute(vector<vector<int>> &sol, const vector<int> &nums, vector<int> &cur, const int i) {
        const int size = nums.size();
        bitset<201> visited;
        for(int j = i; j < size; ++j) {
            int a = nums[j] + 100;
            if(!visited[a] && (cur.empty() || nums[j] >= cur.back())) {
                cur.emplace_back(nums[j]);
                if(cur.size() > 1) sol.emplace_back(cur);
                permute(sol, nums, cur, j+1);
                cur.pop_back();
                visited.set(a);
            }
        }
    } 
};
```
```c++
class Solution {
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        
        if (nums.size() == 1)
            return {};
        
        vector<vector<int>> res;
        vector<int> curr;
            
        dfs(curr, res, 0, nums);
        
        return res;
        
    }
private:
    void dfs(vector<int>& curr, vector<vector<int>>& res, int c_pos, const vector<int>& nums){
        
        // at every depth (which means all curr will have the same size), we maintain a visited set to prevent duplicates (i.e. for [4,6,7,7], we will only get [4,7]). In this way, we won't prevent [4,7,7] as this will be at different depth
        
        unordered_set<int> visited;
        
        for (int i = c_pos; i < nums.size(); i++){
            
            // ----------
            // Step 2. Here we check if we can push the value we are working on to curr or if it is valid to access. Sometimes this step is unncessary, depending on how some of variables in dfs call are changed
            
            // in this question, we have two req before we can push it
            // 1). it is larger than the last of curr
            // 2). it is not used in this depth of call
            if (!curr.empty() && nums[i] < curr.back())
                continue;
            
            if (visited.count(nums[i]))
                continue;
            visited.insert(nums[i]);
            
            // -----------
            // Step 1. This is usually where we start to write this dfs if we are asked to write all possible paths
            // We first write curr.push_back and curr.pop()
            // Then we check if the curr or the pushed value meet some req that makes the curr to be a valid res, if so, we push curr to ans
            // In between we have the recursive call of dfs, with some of its variable updated
            curr.push_back(nums[i]);
            if (curr.size() > 1) 
                res.push_back(curr);
            dfs(curr, res, i+1, nums); // take care of all the results started with i+1
            curr.pop_back();
        }
        
    }
};
```