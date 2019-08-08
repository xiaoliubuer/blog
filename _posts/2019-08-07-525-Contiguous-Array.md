---
layout: post
title:  "525. Contiguous Array"
date: 2019-08-07 20:44:00 -0400
categories: articles
---
Given a binary array, find the maximum length of a contiguous subarray with equal number of 0 and 1.
Example 1:
```
Input: [0,1]
Output: 2
Explanation: [0, 1] is the longest contiguous subarray with equal number of 0 and 1.
```
Example 2:
```
Input: [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
```
Note: The length of the given binary array will not exceed 50,000.
```c++
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        int maxSize = 0, count = 0, size = nums.size(), count0 = 0;
        for(const auto n: nums)
            if(!n) ++count0;
        vector<int> m(size + 1, -2); // Use -2 to mean uninitialized
        
        m[count0] = -1;
        for(int i = 0; i < size; ++i) {
            count += nums[i] == 0 ? -1 : 1;
            const int d = count0 + count;
            if(m[d] != -2) {
                maxSize = max(maxSize, i - m[d]);
            }
            else m[d] = i; 
        }
        return maxSize;
    }
};
```
```c++
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        unordered_map<int,int> indexes;
        int count =0;int max_len = 0;
        indexes[0]=-1;
        for(int i=0;i<nums.size();i++)
        {
            count = count + (nums[i]==0 ?-1:1);
            if(indexes.find(count)==indexes.end())
            {
                indexes[count]=i;
            }
            else
            {
                max_len = max(max_len,i-indexes[count]);
            }
        }
        return max_len;
    }
};
```
```c++

class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        unordered_map<int,int> ma;
        ma[0]=-1;int count1=0;int ans=0;
        for(int i=0;i<nums.size();i++)
        {
            count1+=(nums[i]?1:-1);
            if(ma.count(count1)>0)
            {
                ans=max(ans,i-ma[count1]);
            }
            else
            {
                ma[count1]=i;
            }
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        int maxSize = 0, count = 0, size = nums.size(), count0 = 0;
        
        for(const auto n: nums)
            if(!n) ++count0;

        vector<int> m(size + 1, -2);
        
        m[count0] = -1;
        for(int i = 0; i < size; ++i) {
            count += nums[i] == 0 ? -1 : 1;
            const int d = count0 + count;
            if(m[d] != -2) {
                maxSize = max(maxSize, i - m[d]);
            }
            else m[d] = i; 
        }
        return maxSize;
    }
};
```