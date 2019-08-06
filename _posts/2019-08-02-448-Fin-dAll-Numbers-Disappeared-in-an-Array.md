---
layout: post
title:  "448. Find All Numbers Disappeared in an Array"
date: 2019-08-02 20:44:00 -0400
categories: articles
---
Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements of [1, n] inclusive that do not appear in this array.

Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

Example:
```
Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]
```
```c++

class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> result;        
        for (int i = 0; i < nums.size(); i++) {
            int index = abs(nums[i]) - 1;
            if (nums[index] > 0)
                nums[index] *= -1;
        }
        
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > 0)
                result.push_back(i + 1);
        }
        return result;
    }
};
```
```c++
class Solution {
public:
    
vector<int> findDisappearedNumbers(vector<int>& nums) {
  std::vector<int> ans;
  for (int val : nums) {
    if (nums[abs(val)-1] > 0) nums[abs(val)-1] = -nums[abs(val)-1];
  }
  for (int i = 0; i < nums.size(); ++i) {
    if (nums[i] > 0) {
      ans.push_back(i+1);
    }
  }
  return ans;
}

};
```
```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> ans;
        
        for(int i = 0; i < nums.size(); i++) {
            int index = abs(nums[i]) - 1;
            
            if(nums[index] > 0)
                nums[index] *= -1;
        }
        
        for(int i = 0; i < nums.size(); i++) {
            if(nums[i] > 0) {
                ans.push_back(i+1);
            }
        }
        
        return ans;
    }
};
```
```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int>output;
        for(int i=0; i<nums.size(); i++)
        {
            int flag=(nums[i]>0?nums[i]:(-nums[i]))-1;
                nums[flag]=(nums[flag]<0?nums[flag]:(-nums[flag]));
        }
        for(int i=0; i<nums.size(); i++)
        {
            if(nums[i]>0)
                output.push_back(i+1);
        }
        return output;
    }
};
```