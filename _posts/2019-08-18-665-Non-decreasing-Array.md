---
layout: post
title:  "665. Non-decreasing Array"
date: 2019-08-18 16:30:00 -0400
categories: articles
---
Given an array with n integers, your task is to check if it could become non-decreasing by modifying at most 1 element.

We define an array is non-decreasing if array[i] <= array[i + 1] holds for every i (1 <= i < n).

Example 1:
```
Input: [4,2,3]
Output: True
```
Explanation: You could modify the first 4 to 1 to get a non-decreasing array.

Example 2:
```
Input: [4,2,1]
Output: False
```
Explanation: You can't get a non-decreasing array by modify at most one element.

```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        int cnt = 1, n = nums.size();
        for (int i = 1; i < n; ++i) {
            if (nums[i-1] > nums[i]) {
                if (cnt == 0) return false;
                if (i == 1 || nums[i] >= nums[i - 2]) nums[i - 1] = nums[i]; 
                else nums[i] = nums[i - 1];
                --cnt;
            } 
        }
        return true;
        
    }
};
```
```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        bool chosen = false;
        int prev = -1;
        for(int i =0;i <nums.size()-1;i++)
        {
            if(nums[i]<=nums[i+1])
                continue;
            else
            {
                if(chosen)
                    return false;
                chosen=true;
                if(i==0)
                    continue;
                if(nums[i+1] <= nums[i-1])
                {
                    nums[i+1] = nums[i];
                }
            }
        }
        return true;
    }
};
```
```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        if(nums.size() <= 2) return true;
        for(int i = 1; i < nums.size(); ++i) {
            if(nums[i] < nums[i-1]) {
                return ignore_index_is_possible(i-1, nums) || ignore_index_is_possible(i, nums) || ignore_index_is_possible(i+1, nums);  
            }
        }
        return true;
    }
    
    bool ignore_index_is_possible(int ignore_index, vector<int>& nums) {
         if (ignore_index < 0 || ignore_index >= nums.size()) return false;
         int pre = nums[0];
         int i = 1;
         if(ignore_index == 0) {
             pre = nums[1];
             i = 2;
         }
         for(; i < nums.size(); ++i) {
            if ( i == ignore_index ) {
                continue;
            }
            if(pre > nums[i]) return false;
            pre = nums[i];
         }
        return true;
    }
    
};
```
```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) 
    {
        int editCount = 0;
        if(nums.size() == 1 || nums.size() == 2)
        {
            return true;
        }
        for(int i = 0; i+2 < nums.size(); i++)
        {
            if(nums[i] <= nums[i+1] && nums[i+1] <= nums[i+2])
            {
                continue;
            }
            else if(nums[i] > nums[i+1] && nums[i+1] > nums[i+2])
            {
                return false;
            }
            else
            {
                if(nums[i] <= nums[i+2])
                {
                    nums[i+1] = nums[i];
                    editCount++;
                }
                else if(nums[i+1] <= nums[i+2])
                {
                    nums[i] = nums[i+1];
                    editCount++;
                   
                }
                else if(nums[i] <= nums[i+1])
                {
                    nums[i+2] = nums[i+1];
                    editCount++;
                }
            }
            if(editCount > 1)
            {
                return false;
            }
        }
        return true;
    }
};
```