---
layout: post
title:  "33. Search in Rotated Sorted Array"
date: 2019-06-23 19:13:00 -0400
categories: articles
---
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.
Your algorithm's runtime complexity must be in the order of O(log n).

Example 1:
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```
Example 2:
```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```
```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if ( nums.size() == 0 ) return -1;
        int left = 0;
        int right = nums.size() - 1;
        
        while ( left + 1 < right ) {
            int mid = left + ( right - left )/2;
            
            if( nums[mid] == target ) return mid;
            
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
        
        if ( nums[left] == target ) return left;
        if ( nums[right]== target ) return right;
        return -1;
    }
};
```
```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(nums.empty()) return -1;
        
        int start = 0, end = nums.size() - 1;
        while(start + 1 < end){
            int mid = start + (end - start) / 2;
            
            if (target == nums.at(mid)) return mid;
            if(target == nums.at(start)) return start;
            if(target == nums.at(end)) return end;
            
            if(nums.at(start) < nums.at(mid)){
                //left part
                if (target > nums.at(start) && target < nums.at(mid)) {
                    end = mid;
                } else {
                    start = mid;
                }
            } else {
                if (target < nums.at(end) && target > nums.at(mid)){
                    start = mid;
                } else {
                    end = mid;
                }
            }
        }
        
        if(nums.at(start) == target) return start;
        if(nums.at(end) == target) return end;
        return -1;
    }
};
```