---
layout: post
title:  "34. Find First and Last Position of Element in Sorted Array"
date: 2019-03-23 15:09:23 -0400
categories: articles
---
Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

Example 1:
```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```
Example 2:
```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```
```c++
// Accepted!!!!!!
// This is not right because did not deal with the array length less than 3
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        vector<int> res;
        if ( nums.size() == 0 ) return vector<int>{-1, -1};
        
        while ( left + 1 < right ) {
            int mid = left + ( right - left ) / 2;
            if ( nums[mid] >= target ) right = mid;
            else left = mid;
            // last round: mt t  t / mt mt t / mt t lt / mt mt lt
        }
        if (nums[left] == target) res.push_back(left);
        else if (nums[right] == target) res.push_back(right);
        
        left = 0, right = nums.size() - 1; 
        while ( left + 1 < right ) {
            int mid = left + ( right - left ) / 2;
            if ( nums[mid] <= target ) left = mid;
            else right = mid;
            // last round: 
        }
        if (nums[right] == target) res.push_back(right);
        else if (nums[left] == target) res.push_back(left);

        if ( res.size() == 0 ) return vector<int>{-1, -1};
        return res;
    }
};
```

```c++
// F**k the Damn corner cases!!!!!
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1; 
        int leftEdge = -1, rightEdge = -1;
        
        if ( nums.size() == 0 ) return vector<int>{leftEdge, rightEdge};
        //Find leftEdge
        while ( left < right ) {
            int mid = left + ( right - left ) / 2;
            if ( nums[mid] < target ) left  = mid + 1; //因为这里用了 + 1
            else right = mid - 1; //还有这里用了 - 1， 所以left 或 right 会有越界的可能，所以不用在 binary search 中用 while( left < right ) 这样的设定！！！！ 这样会把你搞得非常被动的！！！！
        }
        
        if ( right >= 0 && nums[right] == target )
        	leftEdge = right;
        else if ( right + 1 < nums.size() && nums[right + 1] == target ) 
        	leftEdge = right + 1;
        
        left = 0; right = nums.size() - 1;
        
        // Find rightEdge
        while ( left < right ) { // [ 2, 2 ]
            int mid = left + ( right - left ) / 2;
            if ( nums[mid] > target ) right = mid - 1;
            else left = mid + 1;
        }
        if ( left < nums.size() && nums[left] == target ) 
        	rightEdge = left;
        else if ( left - 1 >=0 && nums[left - 1] == target ) 
        	rightEdge = left - 1;

        return vector<int>{leftEdge, rightEdge};
    }
};
```