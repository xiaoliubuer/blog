---
layout: post
title:  "219. Contains Duplicate II"
date: 2019-07-27 19:07:00 -0400
categories: articles
---
Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the absolute difference between i and j is at most k.

Example 1:
```
Input: nums = [1,2,3,1], k = 3
Output: true
```
Example 2:
```
Input: nums = [1,0,1,1], k = 1
Output: true
```
Example 3:
```
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```
```c++
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, int> mmap;
        
        for (int i=0; i<nums.size(); i++) {
            if (mmap.find(nums[i]) != mmap.end() && i - mmap[nums[i]] <= k)
                return true;
            mmap[nums[i]] = i;
        }
        
        return false;
    }
};
```

```c++
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, int> map;
        for(int pos = 0; pos < nums.size(); pos++){
            if(map.find(nums[pos]) == map.end()){
                map[nums[pos]] = pos;
            }else {
                if(pos - map[nums[pos]] <= k) return true;
                map[nums[pos]] = pos;
            }
        }
        return false;
    }
};
```


```c++
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        if(nums.size() <= 1) return false;
        unordered_map<int, int> map;
        map[nums[0]] = -1;
        for(int i = 1; i < nums.size(); ++i)
        {
            if(map[nums[i]] != 0)
            {
                if(map[nums[i]] == -1) map[nums[i]]++;
                if(i - map[nums[i]] <= k) return true;
            }
            map[nums[i]] = i;
        }
        
        return false;
    }
};
```

```c++
bool containsNearbyDuplicate(vector<int>& nums, int k) {       
       unordered_set<int> s;
       const int nSize=static_cast<int>(nums.size());      
       
       for (int i = 0; i < nSize; i++) {
           if (i > k) s.erase(nums[i - k - 1]);
           if(!s.insert(nums[i]).second) return true;           
       }
       
       return false;
    }
```