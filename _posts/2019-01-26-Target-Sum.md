---
layout: post
title:  "494. Target Sum"
date: 2019-01-26 11:13:23 -0400
categories: articles
---
You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols + and -. For each integer, you should choose one from + and - as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

Example 1:
```
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```
Note:
```
The length of the given array is positive and will not exceed 20.
The sum of elements in the given array will not exceed 1000.
Your output answer is guaranteed to be fitted in a 32-bit integer.
```
# Funciton signature
```c++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int S) {
        
    }
};
```
# 题意
给一列数和一个target数。只能用 + 或 -，插入到这一列数中间。找到所有可能性使得这一array的总和等于target。
# 想法
就是用一个helper，返回条件是 idx == nums.size(). 同时如果 curr == sum, res++
然后就进行 把当前数 变成+，和变成-的的helper递归。
# 尝试解解
```c++
// Accepted!
class Solution {
public:
	void helper(vector<int>& nums, int idx, int curr, int S, int& res){
		if (idx >= nums.size()) return;
		if (idx == nums.size() - 1){
			if (curr == S){
			res++;
		}
            return;
	}
		helper(nums, idx + 1, curr + abs(nums[idx + 1]), S, res);
		helper(nums, idx + 1, curr - abs(nums[idx + 1]), S, res);
	}
    int findTargetSumWays(vector<int>& nums, int S) {
    	int res = 0;
    	if (nums.size() == 0) return res;
    	helper(nums, 0, nums[0], S, res);
    	helper(nums, 0, -nums[0], S, res);
        return res;
    }
};
```
# 参考答案
```c++
// 内部使用DP
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int s) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        return sum < s || (s + sum) & 1 ? 0 : subsetSum(nums, (s + sum) >> 1); 
    }   

    int subsetSum(vector<int>& nums, int s) {
        int dp[s + 1] = { 0 };
        dp[0] = 1;
        for (int n : nums)
            for (int i = s; i >= n; i--)
                dp[i] += dp[i - n];
        return dp[s];
    }
};
```
