---
layout: post
title:  "16. 3Sum Closest"
date: 2018-11-01 06:55:23 -0400
categories: articles
---
Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

Example:

Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

# 题意: 
给一个array integers，找到三个数的sum最接近target。你可以假设input只有一个solution。

# 思路：

先要有条理的计算三个数的sum，然后找找最接近的存起来。

这个还不是sorted array。 有正有负。

即使sort了以后，如何控制那个指针移动？？？根据什么判断条件？？加入[-4, -1, 1, 2]

看来只能是一个 N乘N的 复杂度。

在和target比较的时候，如何控制left和right指针左右移动呢？？
什么条件下left➡️移？什么条件下right⬅️移？？
```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        if (nums.size() < 3) return 0;
        int res = nums[0] + nums[1] + nums[2];
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size() - 2; ++i){
        	if(i > 0 && nums[i] == nums[i-1]) continue;
        	int left = i+1, right = nums.size() - 1;
        	while( left < right ){
        		int curSum = nums[left] + nums[right] + nums[i];
        		if ( curSum == target) return curSum;
        		//这个地方好重要～～分两步，一个是什么条件给res进行赋值
        		//一个是怎样移动 left 和 right
            	if(abs(target - curSum) < abs( target - res)) {
                	res = curSum;
            	}
            	// 移动left 和 right 的条件
            	if (curSum > target) right--;
            	else left++;
        	}
        }
        return res;
    }
};
```
