---
layout: post
title:  "81. Search in Rotated Sorted Array II"
date: 2019-06-23 19:08:00 -0400
categories: articles
---
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,0,1,2,2,5,6] might become [2,5,6,0,0,1,2]).

You are given a target value to search. If found in the array return true, otherwise return false.

Example 1:
```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```
Example 2:
```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```
Follow up:
```
This is a follow up problem to Search in Rotated Sorted Array, where nums may contain duplicates.
Would this affect the run-time complexity? How and why?
```
```c++
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        if ( nums.size() == 0 ) return false;
        int left = 0;
        int right = nums.size() - 1;
        
        while ( left + 1 < right ) {
  			if ( nums[left] == nums[right]){
  				left++;
  				continue;
  			}
            int mid = left + ( right - left )/2;
            if( nums[mid] == target ) return true;
            if( nums[mid] < target ) {
                if( nums[left] <= nums[mid] )
                    left = mid;
                else {
                    if ( nums[right] >= target )
                        left = mid;
                    else
                        right = mid;
                }
            }
            else { //nums[mid] < target
                if( nums[right] >= nums[mid] )
                    right = mid;
                else { //nums[right] < target
                    if ( nums[left] <= target )
                        right = mid;
                    else
                        left = mid;
                }
            }
        }
        
        if ( nums[left] == target ) return true;
        if ( nums[right]== target ) return true;
        return false;
    }
};
```