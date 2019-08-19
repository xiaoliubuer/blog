---
layout: post
title:  "611. Valid Triangle Number"
date: 2019-08-14 19:44:00 -0400
categories: articles
---	

Given an array consists of non-negative integers, your task is to count the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.
Example 1:
```
Input: [2,2,3,4]
Output: 3
```
Explanation:
```
Valid combinations are: 
2,3,4 (using the first 2)
2,3,4 (using the second 2)
2,2,3
```
Note:
```
The length of the given array won't exceed 1000.
The integers in the given array are in the range of [0, 1000].
```
```c++
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        int res = 0, n = nums.size();
        sort(nums.begin(), nums.end());
        for (int i = n - 1; i >= 2; --i) {
            int left = 0, right = i - 1;
            while (left < right) {
                if (nums[left] + nums[right] > nums[i]) {
                    res += right - left;
                    --right;
                } else {
                    ++left;
                }
            }
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int triangleNumber(vector<int>& nums) 
    {  
        if(nums.size()<3)
            return 0;
        sort(nums.begin(),nums.end());
        int size=nums.size();
        int count=0;
        
        for(int i=2;i<size;i++)
        {
            int l=0, r=i-1;
            while(l<r)
            {
                if(nums[l]+nums[r]>nums[i])
                {
                    count+=r-l;
                    r--;
                }
                else
                    l++;
            }
        }
        
        
        return count;
    }
    
    
};
```
```c++
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        int res = 0, n = nums.size();
        sort(nums.begin(), nums.end());
        for (int i = n - 1; i >= 2; --i) {
            int left = 0, right = i - 1;
            while (left < right) {
                if (nums[left] + nums[right] > nums[i]) {
                    res += right - left;
                    --right;
                } else {
                    ++left;
                }
            }
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        int total = 0;
        sort(nums.begin(), nums.end());
        for (int n=2; n<nums.size(); n++) {
            for (int i=0; i<n-1; i++) {
                if (nums[i] == 0) continue;
                auto it = lower_bound(nums.begin() + i + 1, nums.begin() + n, nums[n] - nums[i] + 1);
                if (it == nums.begin() + n) continue;
                else  total += nums.begin() + n - it; 
            }
        }
        return total;
    }
};
```