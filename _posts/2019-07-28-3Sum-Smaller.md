---
layout: post
title:  "259. 3Sum Smaller"
date: 2019-07-28 16:19:00 -0400
categories: articles
---

Given an array of n integers nums and a target, find the number of index triplets i, j, k with 0 <= i < j < k < n that satisfy the condition nums[i] + nums[j] + nums[k] < target.

Example:
```
Input: nums = [-2,0,1,3], and target = 2
Output: 2 
Explanation: Because there are two triplets which sums are less than 2:
             [-2,0,1]
             [-2,0,3]
```
Follow up: Could you solve it in O(n2) runtime?



```c++
class Solution {
public:
    int threeSumSmaller(vector<int>& nums, int target) {
        if (nums.size() < 3) return 0;
        sort(nums.begin(), nums.end());
        int sum = 0;
        for (int i = 0; i < nums.size() - 2; ++i){
        	sum += twoSumSmaller(nums, i+1, target - nums[i]);
        }
        return sum;
    }
    int twoSumSmaller(vector<int>& nums, int startIndex, int target){
    	int sum = 0;
    	int left = startIndex;
    	int right = nums.size() - 1;
    	while( left < right ){
    		if (nums[left] + nums[right] < target){
    			sum += right - left;
    			left++;
    		}
    		else
    			right--;
    	}
    	return sum;
    }
};
```