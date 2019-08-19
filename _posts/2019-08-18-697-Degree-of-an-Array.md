---
layout: post
title:  "697. Degree of an Array"
date: 2019-08-18 18:20:00 -0400
categories: articles
---
Given a non-empty array of non-negative integers nums, the degree of this array is defined as the maximum frequency of any one of its elements.

Your task is to find the smallest possible length of a (contiguous) subarray of nums, that has the same degree as nums.

Example 1:
```
Input: [1, 2, 2, 3, 1]
Output: 2
Explanation: 
The input array has a degree of 2 because both elements 1 and 2 appear twice.
Of the subarrays that have the same degree:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
The shortest length is 2. So return 2.
```
Example 2:
```
Input: [1,2,2,3,1,4,2]
Output: 6
```
Note:
```
nums.length will be between 1 and 50,000.
nums[i] will be an integer between 0 and 49,999.
```
```c++
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) {
        unordered_map<int,vector<int>>um;
        for (int i=0;i<nums.size();i++){
            um[nums[i]].push_back(i);
        }
        int degree=INT_MIN;
        for (auto i:um){
            degree=max(degree,(int)i.second.size());
        }
        int res=INT_MAX;
        for (auto i:um){
            if (i.second.size()==degree){
                int t=i.second[i.second.size()-1]-i.second[0]+1;
                res=min(res,t);
            }
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) {
        if (nums.size() < 2) return nums.size();
        int res = nums.size();
        unordered_map<int, int> startIndex, counter;
        int len = nums.size();
        int fre = 0;
        for (int i = 0; i < nums.size(); i++){
            if (startIndex.count(nums[i]) == 0) startIndex[nums[i]] = i;
            counter[nums[i]]++;
            if (counter[nums[i]] == fre) {
                len = min(i - startIndex[nums[i]] + 1, len);
            }
            if (counter[nums[i]] > fre) {
                len = i - startIndex[nums[i]] + 1;
                fre = counter[nums[i]];
            }
        }
        return len;
    }
};
```
```c++
class Solution {
public:
  int findShortestSubArray(vector<int>& nums) {
        unordered_map<int, int> counter, first;
        int res = 0, degree = 0;
        for (int i = 0; i < nums.size(); ++i) {
            if (first.count(nums[i]) == 0) first[nums[i]] = i;
            if (++counter[nums[i]] > degree) {
                degree = counter[nums[i]];
                res = i - first[nums[i]] + 1;
            } else if (counter[nums[i]] == degree)
                res = min(res, i - first[nums[i]] + 1);
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) 
    {
        int rank = 1;
        
        map<int ,int>m,occur;
        int distance = INT_MAX;
        //if(rank==1)
        //    return rank;
        
        for(int i=0;i<nums.size();i++)
        {
            m[nums[i]]++;
            
            if(occur[nums[i]]==0)
            {
                occur[nums[i]]=i+1;
            }
            
            else if(m[nums[i]]>=rank)
            {
                
                
                if(m[nums[i]]==rank)
                distance = min(distance,i-occur[nums[i]]+2);
                
                if(m[nums[i]]>rank)
                distance = i-occur[nums[i]]+2;
                
                rank = m[nums[i]];
            }
            
            
            
        }
        
        if(rank==1)
            return 1;
        
        return distance;
        
    }
};
```

```c++
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) {
        if (nums.size() <= 1) return nums.size();
        
        unordered_map<int, vector<int>> m;
        int maxLen = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (m.find(nums[i]) != m.end()) {
                m[nums[i]].push_back(i);
            } else {
                vector<int> tmp{i};
                m[nums[i]] = tmp;
            }
            

            maxLen = max(maxLen, (int)m[nums[i]].size());
        }
        
        
        if (maxLen == 1) return 1;
        
        int size = INT_MAX;
        
        for (auto p : m) {
            
            if (p.second.size() == maxLen) {
                vector<int> v = p.second;
                size = min(size, v[v.size() - 1] - v[0] + 1);
            }
        }
        
        return size;
        
        
    }
};
```