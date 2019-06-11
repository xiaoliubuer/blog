---
layout: post
title:  "217. Contains Duplicate"
date: 2019-06-10 21:25:00 -0400
categories: articles
---

Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

Example 1:
```
Input: [1,2,3,1]
Output: true
```
Example 2:
```
Input: [1,2,3,4]
Output: false
```
Example 3:
```
Input: [1,1,1,3,3,4,3,2,4,2]
Output: true
```

# Best solution

No extra space, O(N)


```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
    if(nums.empty()) return false;
        sort(nums.begin(), nums.end());
        for(int i = 0; i < nums.size() - 1; ++i) {
            if(nums[i] == nums[i+1]) return true;
        }
        return false;
    }
};
```

```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        return nums.size() > set<int>(nums.begin(), nums.end()).size();        
    }
};
```