---
layout: post
title:  "40. Combination Sum II"
date: 2019-01-05 11:04:23 -0400
categories: articles
---
Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

Each number in candidates may only be used once in the combination.

Note:

All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
Example 1:
```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```
Example 2:
```
Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
```
# Function signature
```c++
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        
    }
};
```
# 题意
给一个list的number，找到所有的唯一的组合使得组合的sum等于target
# 想法
基本就是一样的，i 不是从0，而是从idx.

__这种类型的题有一个很重要的就是循环的第一层，和第二层的概念。__
```c++
class Solution {
public:
	void helper(vector<int>& candidates, vector<vector<int>>& res, int idx, int target, vector<int>& curr, vector<int>& visited){
    	if (idx >= candidates.size() || target < 0) return;
    	if (target == 0) res.push_back(curr);

    	for (int i = idx; i < candidates.size(); ++i){
    		if (i > 0 && candidates[i] == candidates[i-1]) continue;
    		curr.push_back(candidates[i]);
    		helper(candidates, res, i + 1, target - candidates[i], curr, visited);
    		curr.pop_back();
    		visited[i] = 1;
    	}
	}

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    	vector<vector<int>> res;
    	if (candidates.size() == 0) return res;
    	vector<int> curr;
    	vector<int> visited(candidates.size(), 0);
    	sort(candidates.begin(), candidates.end());
    	helper(candidates, res, 0, target, curr, visited);
    	return res;
    }
};
```

重要：
在helper里面的那个for循环，每循环一个元素，指的是当前层以这个开头。没进入一个helper是，以这个开头的其他位置的元素的剩余可能性。
所以当第一次调用helper的时候，值得就是curr为空，准备以第一个元素为开头，然后去测试剩余元素，等以index=0的元素为开头的0层curr的剩余元素都被测完之后，就去测试以index=1为开头的元素。

# 参考答案
```c++
// 答案1:
class Solution {
public:
	void helper(vector<int>& candidates, vector<vector<int>>& res, int idx, int target, vector<int>& curr){
    	if (target == 0) {
            res.push_back(curr);
            return;
        }
        for (int i = idx; i < candidates.size() && target >= candidates[i]; ++i){
            if ( i == idx || candidates[i] != candidates[i - 1]){
                curr.push_back(candidates[i]);
                helper(candidates, res, i + 1, target - candidates[i], curr);
                curr.pop_back();
            }
        }
	}
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    	vector<vector<int>> res;
    	if (candidates.size() == 0) return res;
    	vector<int> curr;
    	sort(candidates.begin(), candidates.end());
    	helper(candidates, res, 0, target, curr);
    	return res;
    }
};
```
```c++
class Solution {
public:
	void helper(vector<int>& candidates, vector<vector<int>>& res, int idx, int target, vector<int>& curr){
    	if (target == 0) {
            res.push_back(curr);
            return;
        }
        for (int i = idx; i < candidates.size() && target >= candidates[i]; ++i){
    		if (i > idx && candidates[i] == candidates[i-1]) continue; // OK as well
    		curr.push_back(candidates[i]);
    		helper(candidates, res, i + 1, target - candidates[i], curr);
    		curr.pop_back();
        }
	}
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    	vector<vector<int>> res;
    	if (candidates.size() == 0) return res;
    	vector<int> curr;
    	sort(candidates.begin(), candidates.end());
    	helper(candidates, res, 0, target, curr);
    	return res;
    }
};
```
```c++
// 答案2
class Solution {
public:
    vector<vector<int>> res;
    vector<int> sub;

	void helper(vector<int>& candidates, int target, int index){
		if(target==0) res.push_back(sub);
        else
    		for(int i=index; i<candidates.size(); i++){			
    		    if(target-candidates[i]<0) return;
    			sub.push_back(candidates[i]);
    			helper(candidates, target-candidates[i], i+1);
    			sub.pop_back();
    			while(i+1<candidates.size() && candidates[i] == candidates[i+1]) i++;
    		}
	}

	vector<vector<int>> combinationSum2(vector<int>& candidates, int target){
		if(candidates.size()<1 || target<0) return res;
		sort(candidates.begin(), candidates.end());
		helper(candidates, target, 0);
		return res;
	}
};
```