---
layout: post
title:  "39. Combination Sum"
date: 2019-01-04 21:29:23 -0400
categories: articles
---
Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

The same repeated number may be chosen from candidates unlimited number of times.

Note:

All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
Example 1:
```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```
Example 2:
```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```
# Funciton signature
```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        
    }
};
```
# 题意
从candidates中找到一个组合，他们的和等于target.
# 想法
很重要，这个可以解决所有关于排列，组合的DSF的问题。也就是:
helper函数需要从第一个元素开始算。首先要确定helper函数的退出机制。
```c++
//2019/06/30
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> res;
        vector<int> tmp;
        helper(0, tmp, res, candidates, target);
        return res;
    }
    
    void helper(int idx, vector<int> tmp, vector<vector<int>>& res, vector<int>& candidates, int target ){
        if ( idx >= candidates.size() || target < 0) return;
        if ( target == 0 ) res.push_back(tmp);
        for ( int i = idx; i < candidates.size(); i++ ) {
            tmp.push_back(candidates[i]);
            helper(i, tmp, res, candidates, target - candidates[i]);
            tmp.pop_back();
        }
    }
};
```
```c++
class Solution {
public:
	void helper(vector<int>& candidates, vector<vector<int>>& res, int target, int idx, vector<int>& curr){ // curr, reference or copy??
		if (idx >= candidates.size() || target < 0) return;
		if (target == 0) res.push_back(curr);
		//----------- 这一段代码最重要----------------
		for (int i = idx; i < candidates.size(); ++i) {
			curr.push_back(candidates[i]);
			helper(candidates, res, target - candidates[i], i, curr);
			curr.pop_back();
		}
		//----------- 这一段代码最重要----------------
	}
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    	vector<vector<int>> res;
    	vector<int> curr;
    	if (candidates.size() == 0) return res;
    	helper(candidates, res, target, 0, curr);
    	return res;
    }
};
```

```c++
// Accepted!!!
class Solution {
public:
    
    void helper(vector<int>& candidates, int target, vector<int> temp, vector<vector<int>>& res, int cur, int idx){
        if (cur == target){
            res.push_back(temp);
            return;
        }
        for (int i = idx; i < candidates.size() && cur + candidates[i] <= target; i++){
            vector<int> t = temp;
            t.push_back(candidates[i]);
            cur += candidates[i];
            helper(candidates, target, t, res, cur, i);
            cur -= candidates[i];
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> res;
        
        if (candidates.size() == 0) return res;
        sort(candidates.begin(), candidates.end());
        vector<int> temp;
        helper(candidates, target, temp, res, 0, 0);
        return res;
    }
};
```
这里就涉及到一个问题，是先进行条件满足的判断呢？还是后做这个判断？？
# 参考答案
```c++
class Solution {
public:
	void helper(vector<int>& candidates, vector<vector<int>>& res, int target, int idx, vector<int>& curr){ // curr, reference or copy??
		if (idx >= candidates.size() || target < 0) return;
		if (target == 0) res.push_back(curr);
		for (int i = idx; i < candidates.size(); ++i) {
			curr.push_back(candidates[i]);
			helper(candidates, res, target - candidates[i], i, curr);
			curr.pop_back();
		}

	}
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    	vector<vector<int>> res;
    	vector<int> curr;
    	if (candidates.size() == 0) return res;
    	helper(candidates, res, target, 0, curr);
    	return res;
    }
};
```