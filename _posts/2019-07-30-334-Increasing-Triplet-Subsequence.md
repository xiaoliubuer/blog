---
layout: post
title:  "334. Increasing Triplet Subsequence"
date: 2019-07-30 19:59:00 -0400
categories: articles
---
Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.

Formally the function should:

Return true if there exists i, j, k 
such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.
Note: Your algorithm should run in O(n) time complexity and O(1) space complexity.

Example 1:
```
Input: [1,2,3,4,5]
Output: true
```
Example 2:
```
Input: [5,4,3,2,1]
Output: false
```
```c++
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int seq_one = INT_MAX;
        int seq_two = INT_MAX;
        for(auto num : nums){
            if(num <= seq_one) seq_one = num;
            else if(num <= seq_two) seq_two = num;
            else return true;
        }
        return false;
    }
};
```
```c++
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int m1 = INT_MAX, m2 = INT_MAX;
        for(auto n:nums){
            if(n<=m1){
                m1 = n;
            }
            else if(n<=m2){
                m2 = n;
            }
            else{
                return true;
            }
        }
        return false;
    }
};
```
```c++
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int min1 = INT_MAX;
        int min2 = INT_MAX;
        for(int i = 0; i < nums.size(); i++) {
            if(nums[i] < min1) {
                min1 = nums[i];
            } else if (nums[i] > min1 && nums[i] < min2) {
                min2 = nums[i];
            } else if (nums[i] > min2) {
                return true;
            }
        }
        return false;
    }
};
```
```c++
class Solution {
    public:
        bool increasingTriplet(const vector<int>& nums) {
            int min = INT_MAX;
            int mid = INT_MAX;
            for(auto n : nums){
                if(n < min)
                    min = n;
                else if(n > min){
                    if(mid < n)
                        return true;
                    mid = n;
                }
            }
            return false;
        }
};

```