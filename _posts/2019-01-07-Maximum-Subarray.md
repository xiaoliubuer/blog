---
layout: post
title:  "53. Maximum Subarray"
date: 2019-01-07 21:10:23 -0400
categories: articles
---
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Example:

Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
Follow up:

If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.
# Function signature
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        
    }
};
```
# 题意
一个数组，找到一个子数组它们的和最大，返回最大的那个sum。
# 想法
一般看到最大，最小之类的，就是DP
所以算最大的sum，那么就是
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
    	int n = nums.size();
		vector<int> dp(n, 0); //保存到目前为止最大的sum
        if (n < 1) return 0;
		dp[0] = nums[0];
		int res = dp[0];
		for (int i = 1; i < nums.size(); i++){
			dp[i] = max(nums[i],dp[i-1] + nums[i]);
			res = max(res, max(nums[i], dp[i-1] + nums[i]));
		}
		return res;
    }
};
```
# 参考答案
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if ( nums.size() == 0 ) return 0;
        int res = nums[0];
        
        for ( int i = 1; i < nums.size(); i++ ){
            res = max( res, max( nums[i], nums[i] + nums[i-1]) );
            nums[i] = max( nums[i], nums[i] + nums[i-1]);
        }
        return res;
    }
};
```