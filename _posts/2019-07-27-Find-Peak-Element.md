---
layout: post
title:  "162. Find Peak Element"
date: 2019-07-27 17:17:00 -0400
categories: articles
---

A peak element is an element that is greater than its neighbors.

Given an input array nums, where nums[i] ≠ nums[i+1], find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that nums[-1] = nums[n] = -∞.

Example 1:
```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```
Example 2:
```
Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5 
Explanation: Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.
```
Note:
```
Your solution should be in logarithmic complexity.
```
```c++
class Solution
{
public:
    int findPeakElement(vector<int>& nums) {
    	int l = 0;
    	int r = nums.size()-1;

    	while(l + 1 < r){
    		int mid = (l+r)/2;
    		if ( nums[mid] < nums[mid-1]) //这道题就是让它尽量往中间靠拢。所以只要让它往中间走就行啦
    			r = mid;
    		else if ( nums[mid] < nums[mid+1])
    			l = mid;
    		else
    			// l = mid; 这里等于l还是r无所谓
    			return mid; //这一步是用来处理当
    	}

    	if (nums[l] < nums[r]) //这里的处理是一个重点
    		return r;
    	else
    		return l;
	}	
};
```